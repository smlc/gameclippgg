<script>

	import LoadVid from './LoadVideo/LoadVid.svelte';
	let step 
	function handleMessage(event) {
		alert(event.detail.src);
	}
</script>

<main class="flex h-screen">
	<div class="m-auto">
		<LoadVid on:message={handleMessage}/>
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