
<script>
    import { createEventDispatcher } from 'svelte';

    const dispatch = createEventDispatcher();
    
    function videoLoad(event) {
        let file = this.files[0]
        let fileURL = URL.createObjectURL(file)

        dispatch('message', {
            src: fileURL,
            type: file.type 
		});
        
	}
</script>

<input id="clip" class="bg-gray-100" on:change={videoLoad} type="file" accept="video/*"/>

