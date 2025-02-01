<video autoplay muted loop id="myvideo">
	<source src="/assets/hk.mp4" type="video/mp4">
</video>
<div class="ui">
	<div class="title">
		<img src="/guessr.png">
	</div>
	<div class="selection">
		<img src="/assets/sel.png">
		<a class="btn text" id="launch" href="/game">Play</a>
		<img src="/assets/seli.png">
	</div>
</div>

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