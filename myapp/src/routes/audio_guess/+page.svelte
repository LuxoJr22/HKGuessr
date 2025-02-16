<video autoplay muted loop id="myvideo">
	<source src="/assets/hk.mp4" type="video/mp4">
</video>
<div class="ui">
    <div class="game">
        <audio controls id="audio">
            <source src="" type="audio/mpeg" id="source">
          Your browser does not support the audio element.
        </audio>
        <input type="text" placeholder="Type name ..." id="inp">
    </div>
</div>

<script lang="ts">
    import { afterNavigate } from '$app/navigation';
	afterNavigate(() => { (async () => {

        var obj;
        var i;
        var source = document.getElementById("source") as HTMLSourceElement
        var audio = document.getElementById("audio") as HTMLAudioElement
        var input = document.getElementById("inp") as HTMLInputElement

        async function loadmap()
        {
            obj = await (await fetch("/voices.json")).json();
            i = Math.floor(Math.random() * obj.mob.length);
            source!.src = obj.mob[i].audio;
            audio!.load();
        }
        loadmap()
        
        console.log(input.value)

        input.addEventListener('input', function (e) {
            console.log(input.value)
        });
        
    })();
    });
</script>


<style>
.ui {
    position: absolute;
    top: 0px;
    width: 100%;
    height: 100%;
    display: flex;
    align-items: center;
    justify-content: center;
}

.game {
    display: grid;
}

input[type='text'] { font-size: 16px; font-family: "Trajan"; src: url(../../Trajan_Bold.ttf); height: 35px; margin-top: 10px;}
</style>