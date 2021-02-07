<script>
    import { onMount } from 'svelte';
    export let src;
    export let type;
    let canvasContext;
    let videoNode;

    onMount(async () => {
        videoNode = document.getElementById('video')
        let canPlay = videoNode.canPlayType(type)
        
        if (!canPlay) {
            return
        }
        
        videoNode.src = src

        let canvas = document.getElementById("canvas");
        canvasContext = canvas.getContext("2d");
        let width = 480;
        let height = 270;

        videoNode.onloadedmetadata = () => {
            //Set the current time in order to triger the seeked event.
            videoNode.currentTime = 0;
        }

        videoNode.addEventListener('seeked', function() {

            window.requestAnimationFrame(() => {
                window.requestAnimationFrame(() => {
                    //Draw the first frame
                    canvasContext.drawImage(videoNode, 0, 0, width, height);
                })
            })

        });

        videoNode.addEventListener('canplaythrough', function() {
            setTimeout(videoLoop, 1000 / 30);
        })
        
	});

    function videoLoop() {
        if (videoNode && !videoNode.paused && !videoNode.ended) {
            canvasContext.drawImage(videoNode, 0, 0, 480, 270);
        }
        setTimeout(videoLoop, 1000 / 30);
    }

</script>

<video class="hidden" id="video" controls width="480" height="270" >
    <track default kind="captions"/>
</video>

<div class="min-h-screen m-0 grid grid-rows-3 grid-flow-col gap-4">

    <div class="row-span-3 bg-red-400">
        2
    </div>
        <div class="row-span-2 col-span-2  bg-blue-500 m-auto">
            <canvas id="canvas" width="480" height="270"></canvas>
        </div>
        <div class="col-span-2 bg-green-400"> 
            333
        </div>
</div>



