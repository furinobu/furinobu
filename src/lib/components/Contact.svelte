<script>
	import * as publicEnv from '$env/static/public';
	import { onMount } from 'svelte';

	/** @type {Record<string, string | undefined>} */
	const publicEnvironment = publicEnv;

	/** @param {'PUBLIC_BACKEND_URL' | 'PUBLIC_TURNSTILE_SITE_KEY'} name */
	function requirePublicEnvironmentValue(name) {
		const value = publicEnvironment[name]?.trim();
		if (!value) {
			throw new Error(`Missing required ${name}. Set it before starting or building the frontend.`);
		}
		return value;
	}

	const backendUrl = requirePublicEnvironmentValue('PUBLIC_BACKEND_URL');
	const endpoint = `${backendUrl.replace(/\/+$/, '')}/api/contact`;
	const turnstileSiteKey = requirePublicEnvironmentValue('PUBLIC_TURNSTILE_SITE_KEY');
	const turnstileScriptUrl = 'https://challenges.cloudflare.com/turnstile/v0/api.js?render=explicit';
	/** @typedef {{ render: (container: HTMLElement, options: { sitekey: string, theme: string, size: string, action: string, callback: (token: string) => void, 'error-callback': () => void, 'expired-callback': () => void, 'timeout-callback': () => void }) => string, reset: (widgetId: string) => void, remove: (widgetId: string) => void, isExpired: (widgetId: string) => boolean, getResponse: (widgetId: string) => string }} Turnstile */
	let name = '';
	let email = '';
	let message = '';
	let status = '';
	let verificationStatus = '';
	let turnstileToken = '';
	let isSending = false;
	/** @type {HTMLElement} */
	let turnstileContainer;
	/** @type {string | undefined} */
	let turnstileWidgetId;

	function getTurnstile() {
		return /** @type {Turnstile | undefined} */ (/** @type {any} */ (window).turnstile);
	}

	/** @param {string} message */
	function resetTurnstile(message = '') {
		turnstileToken = '';
		verificationStatus = message;
		const turnstile = getTurnstile();
		if (turnstile && turnstileWidgetId !== undefined) turnstile.reset(turnstileWidgetId);
	}

	/** @param {string} message */
	function verificationFailed(message) {
		resetTurnstile(message);
	}

	function hasValidTurnstileToken() {
		const turnstile = getTurnstile();
		return Boolean(
			turnstileToken &&
			turnstile &&
			turnstileWidgetId !== undefined &&
			!turnstile.isExpired(turnstileWidgetId) &&
			turnstile.getResponse(turnstileWidgetId) === turnstileToken
		);
	}

	onMount(() => {
		const script = document.createElement('script');
		script.src = turnstileScriptUrl;
		script.async = true;
		script.onload = () => {
			const turnstile = getTurnstile();
			if (!turnstile || !turnstileContainer) {
				verificationStatus = 'Verification could not be loaded. Please try again later.';
				return;
			}

			turnstileWidgetId = turnstile.render(turnstileContainer, {
				sitekey: turnstileSiteKey,
				theme: 'dark',
				size: 'flexible',
				action: 'contact',
				/** @param {string} token */
				callback: (token) => {
					turnstileToken = token;
					verificationStatus = 'Verification complete.';
				},
				'error-callback': () => verificationFailed('Verification failed. Please try again.'),
				'expired-callback': () => verificationFailed('Verification expired. Please complete it again.'),
				'timeout-callback': () => verificationFailed('Verification timed out. Please complete it again.')
			});
		};
		script.onerror = () => {
			verificationStatus = 'Verification could not be loaded. Please try again later.';
		};
		document.head.append(script);

		return () => {
			script.onload = null;
			script.onerror = null;
			const turnstile = getTurnstile();
			if (turnstile && turnstileWidgetId !== undefined) turnstile.remove(turnstileWidgetId);
			turnstileWidgetId = undefined;
			script.remove();
		};
	});

	/** @param {SubmitEvent} event */
	async function submitForm(event) {
		event.preventDefault();
		if (isSending) return;

		if (!hasValidTurnstileToken()) {
			resetTurnstile('Complete the verification before sending your message.');
			return;
		}

		isSending = true;
		status = '';
		try {
			const response = await fetch(endpoint, {
				method: 'POST',
				headers: { 'Content-Type': 'application/json', Accept: 'application/json' },
				body: JSON.stringify({ name, email, message, turnstileToken })
			});
			if (!response.ok) throw new Error('The contact service did not accept the message.');
			status = 'Thanks — your message is on its way.';
			name = '';
			email = '';
			message = '';
		} catch (error) {
			status = `Couldn’t send your message right now. Please try again later or configure ${endpoint}.`;
		} finally {
			isSending = false;
			resetTurnstile('Verification reset. Complete it again before sending another message.');
		}
	}
</script>

<section class="section contact" id="contact" aria-labelledby="contact-heading">
	<div class="shell contact-grid"><div><p class="eyebrow">05 — Get in touch</p><h2 class="display section-title" id="contact-heading">Let’s make the next thing interesting.</h2><p class="section-lede">Have a game idea, a technical question, or simply want to swap notes on computers? Send a message.</p><div class="contact-mark" aria-hidden="true">✳</div></div>
		<form class="contact-form glass-card" onsubmit={submitForm}><div class="field"><label for="name">Name</label><input id="name" name="name" autocomplete="name" bind:value={name} required placeholder="Your name" /></div><div class="field"><label for="email">Email</label><input id="email" name="email" type="email" autocomplete="email" bind:value={email} required placeholder="you@example.com" /></div><div class="field"><label for="message">Message</label><textarea id="message" name="message" bind:value={message} required rows="5" placeholder="What’s on your mind?"></textarea></div><div class="verification"><p class="verification-label" id="verification-label">Human verification</p><div class="turnstile-widget" aria-labelledby="verification-label" bind:this={turnstileContainer}></div><p class="verification-status" id="verification-status" class:error={verificationStatus.includes('unavailable') || verificationStatus.includes('failed') || verificationStatus.includes('expired') || verificationStatus.includes('timed out') || verificationStatus.includes('could not')} aria-live="polite">{verificationStatus}</p></div><button class="send" type="submit" disabled={isSending} aria-describedby="verification-status">{isSending ? 'Sending…' : 'Send message'} <span aria-hidden="true">↗</span></button><p class="status" class:error={status.startsWith('Couldn')} aria-live="polite">{status}</p></form>
	</div>
</section>

<style>
	.contact { padding-bottom: 7rem; } .contact-grid { display: grid; grid-template-columns: .9fr 1.1fr; gap: 5rem; } .contact-mark { color: var(--color-aqua); font-size: 5rem; line-height: 1; margin-top: 2rem; opacity: .75; } .contact-form { border-radius: 1.25rem; padding: 1.5rem; } .field + .field { margin-top: 1rem; } label, .verification-label { color: #dbe5ff; display: block; font-size: .8rem; font-weight: 700; margin-bottom: .45rem; } input, textarea { background: rgba(5,8,20,.46); border: 1px solid var(--color-line); border-radius: .65rem; color: white; display: block; padding: .78rem .85rem; resize: vertical; transition: border-color 160ms ease, background 160ms ease; width: 100%; } input::placeholder, textarea::placeholder { color: #657399; } input:hover, textarea:hover { border-color: rgba(197,213,255,.28); } input:focus, textarea:focus { background: rgba(5,8,20,.7); border-color: var(--color-aqua); outline: 0; } .verification { margin-top: 1rem; } .turnstile-widget { min-height: 65px; } .verification-status { color: var(--color-aqua); font-size: .82rem; line-height: 1.45; min-height: 1.2rem; margin: .45rem 0 0; } .send { background: var(--color-aqua); border: 0; border-radius: .65rem; color: #041714; font-weight: 800; margin-top: 1.2rem; padding: .82rem 1rem; transition: transform 160ms ease, opacity 160ms ease; width: 100%; } .send:hover:not(:disabled) { transform: translateY(-2px); } .send:disabled { cursor: wait; opacity: .7; } .status, .verification-status { color: var(--color-aqua); font-size: .82rem; line-height: 1.45; min-height: 1.2rem; margin: .8rem 0 0; } .status.error, .verification-status.error { color: #ffb6af; } @media (prefers-reduced-motion: reduce) { input, textarea, .send { transition: none; } .send:hover:not(:disabled) { transform: none; } } @media (max-width: 800px) { .contact-grid { grid-template-columns: 1fr; gap: 2rem; } .contact-mark { display: none; } }
</style>
