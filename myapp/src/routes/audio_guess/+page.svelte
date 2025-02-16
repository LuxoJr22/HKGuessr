<video autoplay muted loop id="myvideo">
	<source src="/assets/hk.mp4" type="video/mp4">
</video>
<div class="ui">
    <div class="game" id="game">
        <audio controls id="audio">
            <source src="" type="audio/mpeg" id="source">
          Your browser does not support the audio element.
        </audio>
        <input type="text" autofocus placeholder="Type name ..." id="inp" autocomplete="new-password">
        <div class="tab" id="tab">            
        </div>
    </div>
</div>

<script lang="ts">
    import { afterNavigate } from '$app/navigation';
	afterNavigate(() => { (async () => {

        var obj;
        var i : number;
        var source = document.getElementById("source") as HTMLSourceElement
        var audio = document.getElementById("audio") as HTMLAudioElement
        var input = document.getElementById("inp") as HTMLInputElement
        var tab = document.getElementById("tab")
        var game = document.getElementById("game")
        var names : string[] = []
        var icons : string[] = []
        var elements : HTMLDivElement[] = []
        var el : HTMLDivElement;

        async function loadmap()
        {
            obj = await (await fetch("/voices.json")).json();
            
            for (let m in obj.mob){
                names.push(obj.mob[m].name);
                icons.push(obj.mob[m].img)
            }
            
            
            i = Math.floor(Math.random() * obj.mob.length);
            source!.src = obj.mob[i].audio;
            audio!.load();
        }

        function get_names()
        {
            tab?.replaceChildren()
            if (input.value == "")
                return
            for (let i in names) {
                if (names[i].toLowerCase().indexOf(input.value.toLowerCase()) == 0) {
                    el = document.createElement('div')
                    el.classList.add("box")
                    el.onclick = function() {guess_value(names[i]); tab?.replaceChildren()};
                    let img = document.createElement('img')
                    img.src = icons[i]
                    img.classList.add("image")
                    el.appendChild(img)
                    let txt = document.createElement('p')
                    txt.textContent = names[i]
                    txt.classList.add("text")
                    el.appendChild(txt)
                    tab?.append(el)
                    let line = document.createElement('div')
                    line.classList.add("select_line")
                    tab!.append(line)
                    elements.push(el)
                }
            };
        }

        function guess_value(n : string) {
            if (n.toLowerCase() == names[i].toLowerCase()) {
                game?.replaceChildren()
                let txt = document.createElement('p')
                txt.textContent = "Congrats"
                txt.classList.add("text")
                game?.appendChild(txt)
            }
        }


        loadmap()

        input.addEventListener('input', function (e) {
            get_names()
        });

        input.addEventListener('keydown', function (e) {
            if (e.keyCode == 13) {
                guess_value(input.value)
                input.value = "";
                
            }

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

.tab {
    max-height: 12em;
    overflow-y: auto;
    --sb-track-color: #232E33;
    --sb-thumb-color: #ffffff;
    --sb-size: 14px;
    backdrop-filter: blur(10px);
}

:global(.box) {
    display: flex;
    align-items: center;
    font-size: 16px;
    font-family: "Trajan";
    src: url(../../Trajan_Bold.ttf);
    color: #E6E6E6;
    margin: 1% 0 1% 0;
}

:global(.select_line) {
	height: 1px;
    background-color: #E6E6E6;
    width: 100%;
    margin: auto 0;
	mask-image: linear-gradient(to left, transparent, black 45%, black 55%, transparent);
}

:global(.text) {
    font-size: 16px;
    font-family: "Trajan";
    src: url(../../Trajan_Bold.ttf);
    color: #E6E6E6;
}

:global(.image) {
    width: 3em;
    border-radius: 50%;
    object-fit: cover;
    aspect-ratio: 1;
}

input[type='text'] {
    font-size: 16px;
    font-family: "Trajan";
    src: url(../../Trajan_Bold.ttf);
    height: 35px;
    margin: 10px 0 10px 0;
    padding: 5px 10px;
}

.tab::-webkit-scrollbar {
  width: var(--sb-size)
}

.tab::-webkit-scrollbar-track {
  background: var(--sb-track-color);
  border-radius: 4px;
}

.tab::-webkit-scrollbar-thumb {
  background: var(--sb-thumb-color);
  border-radius: 4px;
  
}

</style>