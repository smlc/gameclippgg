<script>

	import LoadVid from './LoadVideo/LoadVid.svelte';
	import VideoCanvas from './VideoCanvas/VideoCanvas.svelte';
	let step = 1;
	let videoInfo = {
		src: '',
		type: ''
	};
	function handleMessage(event) {
		videoInfo.src = event.detail.src;
		videoInfo.type = event.detail.type;
		step = 2;
	}
</script>

<main>
	<div >
		{#if step == 1}
			<LoadVid on:message={handleMessage}/>
		{:else if step == 2}
			<VideoCanvas {...videoInfo}/>
		{:else}
			<p>Nothing</p>
		{/if}
		
	</div>
</main>

<style global lang="postcss">
  /* only apply purgecss on utilities, per Tailwind docs */
  /* purgecss start ignore */
  @tailwind base;
  @tailwind components;
  /* purgecss end ignore */
 
  @tailwind utilities;

</style>