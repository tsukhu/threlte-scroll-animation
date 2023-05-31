<script>
	import '../app.css';
</script>

<slot />

<style>
	:global(body) {
		@apply bg-gray-800 text-gray-100 px-4;
		overflow-x: hidden;
	}
	:global(canvas) {
		height: 100%;
	}
</style>
