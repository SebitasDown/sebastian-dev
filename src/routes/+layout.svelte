<script lang="ts">
	import '../app.css';
	import favicon from '$lib/assets/favicon.svg';

	let { children } = $props();
</script>

<svelte:head>
	<link rel="icon" href={favicon} />
	<meta name="description" content="Sebastian Mazo — Desarrollador Full-Stack. Creando experiencias digitales con tecnologías modernas." />
	<meta name="author" content="Sebastian Mazo" />
	<meta property="og:title" content="Sebastian Mazo — Portafolio" />
	<meta property="og:description" content="Desarrollador Full-Stack especializado en aplicaciones web y móviles modernas." />
</svelte:head>

{@render children()}
