<video autoplay muted loop id="myvideo">
	<source src="/assets/hk.mp4" type="video/mp4">
</video>
<div class="ui">
	<div class="title">
		<img src="/guessr.png" alt="title">
	</div>
	<div class="selection">
		<img src="/assets/sel.png" alt="Play btn">
		<a class="btn text" id="launch" on:click={() => myGoto("/game")}>Play</a>
		<img src="/assets/seli.png" alt="Play btn">
	</div>
	<div class="selection">
		<img src="/assets/sel.png" alt="Play btn">
		<a class="btn text" id="launch" on:click={() => myGoto("/audio_guess")}>Audio Guess</a>
		<img src="/assets/seli.png" alt="Play btn">
	</div>
</div>

<script lang="ts">
	
	function myGoto(href : string){
		window.location.href = href
	}

</script>

<style>

.ui {
	position: absolute;
	height: 80%;
	width: 100%;
	top: 0px;
	display:flex;
	justify-content: space-evenly;
	flex-direction: column;
	align-items: center;
}

a.text {
	color: white;
	font-size: 50px;
	font-family: "Trajan";
	text-decoration: none;
}

a.text:hover {
	text-shadow: 0 0px 4px white;
}

.title {
	width: 40%;
	height: auto;
}

div.title > img {
	width: 100%;
	height: auto;
}
</style>