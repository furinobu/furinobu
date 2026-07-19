<script>
	let menuOpen = false;
	const links = [
		['About', '#about'], ['Skills', '#skills'], ['Projects', '#projects'], ['Education', '#education'], ['Contact', '#contact']
	];

	function closeMenu() { menuOpen = false; }
</script>

<header class="nav-wrap">
	<nav class="shell nav" aria-label="Main navigation">
		<a class="brand display" href="#home" aria-label="Harry — home" onclick={closeMenu}>H<span>.</span></a>
		<button class="menu-button" type="button" aria-label={menuOpen ? 'Close navigation menu' : 'Open navigation menu'} aria-expanded={menuOpen} aria-controls="nav-links" onclick={() => menuOpen = !menuOpen}>
			<span></span><span></span>
		</button>
		<div class:open={menuOpen} class="nav-links" id="nav-links">
			{#each links as [label, href]}
				<a class="nav-link" href={href} onclick={closeMenu}>{label}</a>
			{/each}
			<a class="nav-contact" href="#contact" onclick={closeMenu}>Let's talk <span aria-hidden="true">↗</span></a>
		</div>
	</nav>
</header>

<style>
	.nav-wrap { position: fixed; z-index: 30; top: 0; width: 100%; border-bottom: 1px solid rgba(197,213,255,.1); background: rgba(8,11,24,.72); backdrop-filter: blur(18px); }
	.nav { height: 4.5rem; display: flex; align-items: center; justify-content: space-between; }
	.brand { font-size: 1.5rem; font-weight: 700; letter-spacing: -.08em; } .brand span { color: var(--color-aqua); }
	.nav-links { display: flex; align-items: center; gap: 1.6rem; }
	.nav-link { color: var(--color-muted); font-size: .88rem; font-weight: 650; transition: color 160ms ease; } .nav-link:hover { color: white; }
	.nav-contact { background: rgba(82,225,207,.12); border: 1px solid rgba(82,225,207,.3); border-radius: 999px; color: var(--color-aqua); font-size: .84rem; font-weight: 750; padding: .56rem .82rem; }
	.menu-button { display: none; background: transparent; border: 0; padding: .4rem; } .menu-button span { width: 1.25rem; height: 1px; background: white; display: block; margin: .27rem; }
	@media (max-width: 720px) { .menu-button { display: block; } .nav-links { display: none; position: absolute; top: 4.5rem; left: 1rem; right: 1rem; padding: 1rem; border: 1px solid var(--color-line); border-radius: 1rem; background: #11162b; box-shadow: 0 20px 40px rgba(0,0,0,.3); align-items: stretch; flex-direction: column; gap: 1rem; } .nav-links.open { display: flex; } .nav-contact { text-align: center; } }
</style>
