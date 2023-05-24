<script lang="ts">
	import MainScene from '../components/MainScene.svelte';
	import { scrollPercent } from '../stores/scrollStore';

	document.body.onscroll = () => {
		//calculate the current scroll progress as a percentage
		scrollPercent.update(
			(_) =>
				((document.documentElement.scrollTop || document.body.scrollTop) /
					((document.documentElement.scrollHeight || document.body.scrollHeight) -
						document.documentElement.clientHeight)) *
				100
		);
	};
</script>

<span class="fixed bottom-2 left-2 z-50 text-base"
	>{'Scroll Progress : ' + $scrollPercent.toFixed(2)}</span
>
<main class="w-screen h-screen font-sans absolute justify-center items-center">
	<h1 class="mx-auto text-4xl font-semibold text-center mt-4">Threlte Scroll Animation</h1>
	<section class="min-h-screen p-5 text-2xl text-center">
		<h2>Begin scrolling to see things change</h2>
	</section>
	<section class="min-h-screen p-5 text-2xl text-center">
		<h2>Changing Objects Position</h2>
		<p>The cubes position is now changing</p>
	</section>

	<section class="min-h-screen p-5 text-2xl text-center">
		<h2>Changing Objects Rotation</h2>
		<p>The cubes rotation is now changing</p>
	</section>

	<section class="min-h-screen p-5 text-2xl text-center">
		<h2>Changing Camera Position</h2>
		<p>The camera position is now changing</p>
	</section>

	<section class="min-h-screen p-5 text-2xl text-center">
		<h2>You are at the bottom</h2>
		<p>The cube will now be auto rotating</p>
		<p>Now you can scroll back to the top to reverse the animation</p>
	</section>
	<MainScene />
</main>
