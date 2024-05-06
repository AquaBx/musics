<script lang="ts">
    import Library from "$lib/library.svelte";
    import { index } from "$lib/store.js";
    import { onMount } from "svelte";

    let {data} = $props()
    let actualiseBarInterval

    var audio:HTMLAudioElement
    var isPlaying = false
    var percent = $state(0)

    onMount(() => {
        audio = new Audio(data.library.at($index));
        audio.onended = next
        audio.onplaying = () => {
            isPlaying = true
            actualiseBarInterval = setInterval(actualiseBar,1000)
        }

        audio.onpause = () => {
            isPlaying = false
        }
    })

    index.subscribe(()=>{
        if (audio == undefined ) return
        audio.src = data.library.at($index)
        audio.play()
    })

    function playPause() {
        if (audio.paused) audio.play()
        else audio.pause()
    }

    function previous(){
        audio.pause()
        $index = ($index - 1)%data.library.length
    }

    function next(){
        audio.pause()
        $index = ($index + 1)%data.library.length
    }

    function actualiseBar(){
        percent = audio.currentTime / audio.duration
        if (!isPlaying) clearInterval(actualiseBarInterval)
    }
    
</script>

<div class="max-w-screen-md border border-slate-200 dark:border-slate-600">

    <Library lib={data.library}></Library>
    <div class="bg-white dark:bg-slate-800">
        <div class="dark:bg-slate-700 bg-slate-200 h-2" style="width:{percent*100}%;"></div>
    </div>
    <div class="flex justify-center gap-2 bg-white dark:bg-slate-800 text-slate-500 dark:text-slate-400">
        <button class=" p-2" onclick={previous}><i class="ph ph-skip-back"></i></button>
        <button class=" p-2" onclick={playPause}><i class="ph ph-play"></i></button>
        <button class=" p-2" onclick={next}><i class="ph ph-skip-forward"></i></button>
    </div>
        
</div>
