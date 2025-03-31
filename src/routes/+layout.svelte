<script>
	import '../app.css';
	let { children } = $props();
</script>

<div class="layout">
	{@render children()}
</div>

<style>
	.layout {
		max-width: 32rem;
		margin: 0 auto;
		padding: 6rem 2rem;
	}

	@media (min-width: 1024px) {
		.layout {
			padding-left: 0;
			padding-right: 0;
		}
	}
</style>
