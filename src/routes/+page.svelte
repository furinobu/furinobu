<script>
	import Navigation from '$lib/components/Navigation.svelte';
	import Hero from '$lib/components/Hero.svelte';
	import About from '$lib/components/About.svelte';
	import Skills from '$lib/components/Skills.svelte';
	import Projects from '$lib/components/Projects.svelte';
	import Education from '$lib/components/Education.svelte';
	import Contact from '$lib/components/Contact.svelte';
	import Footer from '$lib/components/Footer.svelte';
</script>

<svelte:head>
	<title>Harry — Building curious things</title>
	<meta property="og:title" content="Harry — Information Engineering student at CUHK" />
	<meta property="og:description" content="A portfolio of software, game programming, and curious computer projects." />
</svelte:head>

<Navigation />
<main>
	<Hero />
	<About />
	<Skills />
	<Projects />
	<Education />
	<Contact />
</main>
<Footer />
