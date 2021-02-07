<script>
	import Nav from '../components/Nav.svelte';
	import GoogleAnalytics from '../components/GoogleAnalytics.svelte';
	export let segment;
</script>

<style>
	.container {
		position: absolute;
		display: flex;
		align-items: center;
		justify-content: center;
		width: 100%;
		height: 88%;
		top: 9%;
		left: 0;
	}
</style>

<GoogleAnalytics />
<Nav {segment}/>

<main class='container'>
	<slot></slot>
</main>