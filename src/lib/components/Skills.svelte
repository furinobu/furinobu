<script>
	const groups = [
		{ label: 'Web', detail: 'Ideas that meet the browser', color: '#52e1cf', skills: [{ name: 'HTML5', level: 88 }, { name: 'CSS3', level: 82 }, { name: 'JavaScript', level: 76 }] },
		{ label: 'Software', detail: 'Tools, systems, and play', color: '#a78bfa', skills: [{ name: 'Python', level: 83 }, { name: 'C / C++', level: 76 }, { name: 'Rust', level: 68 }, { name: 'Java', level: 70 }, { name: 'Pascal', level: 65 }] }
	];
</script>

<section class="section skills-section" id="skills" aria-labelledby="skills-heading">
	<div class="shell"><p class="eyebrow">02 — Toolkit</p><h2 class="display section-title" id="skills-heading">Different languages.<br />One habit: make it work.</h2><p class="section-lede">A growing toolkit for building on the web, shaping software, and experimenting with the rules that make games feel alive.</p>
		<div class="skill-groups">{#each groups as group}<article class="skill-group glass-card" style={`--accent: ${group.color}`}><header><span class="group-index">{group.label === 'Web' ? 'A' : 'B'}</span><div><h3>{group.label}</h3><p>{group.detail}</p></div></header><ul>{#each group.skills as skill}<li><div class="skill-label"><span>{skill.name}</span><span>{skill.level}%</span></div><div class="track" aria-label={`${skill.name} familiarity: ${skill.level} out of 100`} role="meter" aria-valuenow={skill.level} aria-valuemin="0" aria-valuemax="100"><span style={`--level: ${skill.level}%`}></span></div></li>{/each}</ul></article>{/each}</div>
	</div>
</section>

<style>
	.skills-section { background: linear-gradient(180deg, transparent, rgba(36,50,94,.16), transparent); } .skill-groups { display: grid; grid-template-columns: repeat(2,1fr); gap: 1.2rem; margin-top: 3rem; } .skill-group { border-radius: 1.25rem; padding: 1.5rem; } .skill-group header { display: flex; align-items: center; gap: 1rem; border-bottom: 1px solid var(--color-line); padding-bottom: 1.25rem; } .group-index { display: grid; width: 2.1rem; height: 2.1rem; place-items: center; border: 1px solid color-mix(in srgb, var(--accent), transparent 50%); border-radius: .6rem; color: var(--accent); font-family: var(--font-display); font-weight: 700; } h3 { font-family: var(--font-display); font-size: 1.3rem; margin: 0; } header p { color: var(--color-muted); font-size: .82rem; margin: .13rem 0 0; } ul { list-style: none; padding: 0; margin: 1.4rem 0 0; } li + li { margin-top: 1rem; } .skill-label { display: flex; justify-content: space-between; color: #c6d2ee; font-size: .85rem; font-weight: 650; margin-bottom: .45rem; } .skill-label span + span { color: var(--color-muted); font-size: .73rem; } .track { background: rgba(200,215,255,.09); border-radius: 999px; height: .34rem; overflow: hidden; } .track span { display: block; background: var(--accent); border-radius: inherit; height: 100%; width: var(--level); box-shadow: 0 0 12px color-mix(in srgb, var(--accent), transparent 50%); animation: fill 950ms cubic-bezier(.2,.8,.2,1) both; transform-origin: left; } @keyframes fill { from { transform: scaleX(0); } to { transform: scaleX(1); } } @media (max-width: 720px) { .skill-groups { grid-template-columns: 1fr; } }
</style>
