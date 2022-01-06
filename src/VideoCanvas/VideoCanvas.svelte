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
            let progressBar = document.getElementById('progress-bar')
            progressBar.setAttribute('max', Math.round(videoNode.duration));
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
        video.addEventListener('timeupdate', updateProgressBar);
        
	});

    const videoLoop = () => {
        if (videoNode && !videoNode.paused && !videoNode.ended) {
            canvasContext.drawImage(videoNode, 0, 0, 480, 270);
        }
        setTimeout(videoLoop, 1000 / 30);
    }

    const playVideo = () => {
        if (video.paused || video.ended) {
            video.play();
            
        } else {
            video.pause();
        }

        const playbackIcons = document.querySelectorAll('.playback-icons use');
        playbackIcons.forEach(icon => icon.classList.toggle('hidden'));
    }

    const updateProgressBar = (event) => {
       //let pos = (e.pageX  - (this.offsetLeft + this.offsetParent.offsetLeft)) / this.offsetWidth;
       // video.currentTime = pos * video.duration;
       let progressBar = document.getElementById('progress-bar');
       progressBar.value = Math.floor(videoNode.currentTime);
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
    <div class="col-span-2 w-full bg-green-400"> 
        <div id="video-controller w-full" class="flex">
            <button on:click={playVideo} on:click={updateProgressBar} class="inline-flex items-center border-0">
                <svg class="playback-icons w-10 h-10" fill="none" stroke="currentColor" >
                    <use id="playIconSvg" href="#play-icon"></use>
                    <use id="pauseIconSvg" class="hidden" href="#pause-icon"></use>
                  </svg>
            </button>
            <div class="video-progress w-full relative mt-2">
                <progress class="absolute appearance-none pointer-events-none w-10/12 bg-gray-500 top-0 h-4 border-0 rounded " id="progress-bar" value="0" min="0"></progress>
            </div>
        </div>
    </div>
</div>


<svg style="display: none">

    <defs>
        <symbol id="play-icon" viewBox="0 0 24 24">
            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M14.752 11.168l-3.197-2.132A1 1 0 0010 9.87v4.263a1 1 0 001.555.832l3.197-2.132a1 1 0 000-1.664z"></path><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M21 12a9 9 0 11-18 0 9 9 0 0118 0z"></path>
        </symbol>
    </defs>

    <defs>
        <symbol id="pause-icon" viewBox="0 0 24 24">
        <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M10 9v6m4-6v6m7-3a9 9 0 11-18 0 9 9 0 0118 0z"></path>        </symbol>
    </defs>

</svg>


