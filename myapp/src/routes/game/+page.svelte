<div class="feature_image" id="place">
	<img id="point" alt="location">
</div>
<div class="top_ui" id="top">
	<span class="text" id="Score">Score: 0</span>
	<span class="text" id="Timer">TIMER</span>
	<span class="text" id="round">Round : 0/5</span>
</div>
<div class="guess_ui" id="guess">
	<div class="icons" id="maps_btns">	
		<div class="select" id="select_hud">
			<div class="select_line"></div>
			<div class="select_back"></div>
			<div class="select_line"></div>
		</div>
		<div class="icon" id="Hallowseal">
			<img src="/assets/hallowseal.png" alt="hallowseal">
		</div>
		<div class="icon" id="Godmaster">
			<img src="/assets/godmaster.png" alt="godmaster">
		</div>
		<div class="icon" id="King_brand">
			<img src="/assets/Kings_Brand_inventory.webp" alt="king">
		</div>
	</div>
	<div class="map_ui" id ="map">
		<div class="zoom_outer" id="mapping">
			<div id="tag" hidden>
				<img src="/assets/knight.png" alt="knight">
			</div>
			<div id="tagpoint" hidden>
				<img src="/Wayward_Compass.webp" alt="compass">
			</div>
			<svg height="100%" width="100%" id="box">
				<line x1="0" y1="0" x2="400" y2="200" stroke-dasharray="4" id="line" />
			</svg>
			<div id="zoom">
				<img src="/assets/map.png" alt="zoom" id="Hallownest">
				<img src="/assets/Godhome_Mapa.webp" alt="zoom" id="Godhome" hidden>
				<img src="/assets/White_Palace_Map_Clean.png" alt="zoom" id="White_Palace" hidden>
			</div>
		</div>
	</div>
	<div class="mybutton">
		<div class="guessimgs" id="guess_text">
			<img src="/assets/guessbtnl.png" alt="guess btn">
			<img src="/assets/guessbtn.png" alt="guess btn" class="img-top" id="btn">
		</div>
	</div>
</div>
<div class="bot_ui" id="bot">
	<span class="text" id="Act_score">Score: 0</span>
	<div class="nextimgs" id="next_text">
		<img src="/assets/Nextl.png" alt="Next btn">
		<img src="/assets/Next.png" alt="Next btn" class="img-top" id="next">
	</div>
	<div class="nextimgs" id="playa_text">
		<img src="/assets/playal.png" alt="Play again btn">
		<img src="/assets/playa.png" alt="Play again btn" class="img-top" id="playa">
	</div>
	<span class="text" id="Act_round">Round : 1/5</span>
</div>

<script lang="ts">
    import { onMount } from "svelte";
	onMount(() => { (async () => {
	var scale = 1
	var totalSeconds : number
	var chrono : number
	var panning = false,
	pointX = 0,
	pointY = 0,
	start = { x: 0, y: 0 },
	pos = {x: 0, y: 0},
	tagpos = {x: 0, y: 0},
	point = {x: 0, y: 0},
	pospoint = {x:0, y:0},
	tagpoint = {x:0, y:0},
	score = 0,
	Round = 1,
	scene = 0,
	totalsec = 0
	var gamepoints : number[] = []
	var tags : HTMLElement[] = []
	var points : HTMLElement[] = []
	var lines : HTMLElement[] = []
	var mappoints : {x : number, y : number}[] = []
	var maptags : {x : number, y : number}[] = []
	var act_map = "Hallownest"
	var pos_map = ""
	var maps : string[] = []

	
	
	var bot = document.getElementById("guess"),
	bott = document.getElementById("bot"),
	map = document.getElementById("map"),
	act_score = document.getElementById("Act_score"),
	act_round = document.getElementById("Act_round"),
	zoom = document.getElementById("zoom"),
	locate = document.getElementById("point") as HTMLImageElement,
	taupe = document.getElementById("top"),
	tag = document.getElementById("tag"),
	tagpo = document.getElementById("tagpoint"),
	place = document.getElementById("place"),
	timer = document.getElementById("Timer"),
	button = document.getElementById("btn"),
	scoretxt = document.getElementById("Score"),
	round = document.getElementById("round"),
	guess_imgs = document.getElementById("guess_text"),
	next = document.getElementById("next_text"),
	playa = document.getElementById("playa_text"),
	line = document.getElementById("line"),
	box = document.getElementById("box"),
	map_btns = document.getElementById("maps_btns"),
	h_map = document.getElementById("Hallownest"),
	w_map = document.getElementById("White_Palace"),
	g_map = document.getElementById("Godhome"),
	h_icon = document.getElementById("Hallowseal"),
	k_icon = document.getElementById("King_brand"),
	g_icon = document.getElementById("Godmaster"),
	select = document.getElementById("select_hud")
	var rect = zoom!.getBoundingClientRect();


	function get_rect()
	{
		pointX = 0;
		pointY = 0;
		start = { x: 0, y: 0 };
		pos = {x: 0, y: 0};
		scale = 1;
		zoom!.style.transform = "translate(" + pointX + "px, " + pointY + "px) scale(" + scale + ")";
		rect = zoom!.getBoundingClientRect();
	}


	function loadmap()
	{
		scene = 0;
		clearInterval(chrono);
		StartTimer();
		fetch('/map.json')
			.then((response) => response.json())
			.then((json) => {
				var i = json.maps.length;
				var o = Math.floor(Math.random() * i);
				while (gamepoints.includes(o))
				{
					o = (o + 1) % i;
				}
				gamepoints.push(o);
				var p = json.maps[o];
				//locate!.src = "/assets/point/wp1.png";
				locate!.src = p.link;
				pospoint = p.coords;
				pos_map = p.map});
		scoretxt!.innerHTML = "Score : " + score;
		round!.innerHTML = "Round : " + Round + "/5";
	}

	function reloadmap()
	{
		act_map = "Hallownest";
		show_map(act_map)
		select!.style.left = "0px";
		loadmap();
		map_btns!.style.display = "flex";
		map!.style.backgroundColor =  "white";
		bot!.style.right = 20 + "px";
		bot!.style.top = "";
		bot!.style.bottom = 80 + "px";
		map!.style.opacity = "0.5";
		map!.style.height = "auto";
		map!.style.width = 600 + "px";
		tagpos.x = 0;
		tagpos.y = 0;
		tag!.style.left = tagpos.x + "px";
		tag!.style.top = tagpos.y + "px";
		setTransform()
		tag!.hidden = true;
		tagpo!.hidden = true;
		taupe!.style.display = "flex";
		guess_imgs!.style.display = "flex";
		bott!.style.display = "none";
		box!.style.display = "none"
		get_rect();
	}

	function guess_scene()
	{
		if (pos_map != act_map)
		{
			tag!.hidden = true;
			tagpos = {x: 0, y: 0};
		}
		totalsec += 300 - totalSeconds;
		scene = 1;
		map!.style.backgroundColor =  "black";
		map_btns!.style.display = "none";
		bot!.style.right = 0 + "px";
		bot!.style.top = 0 + "px";
		map!.style.opacity = "1";
		map!.style.height = 90 + "%";
		map!.style.width = "auto";
		get_rect();
		tagpos.x *= ((rect.right - rect.left) / 600);
		tagpos.y *= ((rect.bottom - rect.top) / 375);
		tag!.style.left = tagpos.x + "px";
		tag!.style.top = tagpos.y + "px";
		tag!.style.transform = "translate(" + 0 + "px, " + 0 + "px)";
		tagpo!.style.transform = "translate(" + 0 + "px, " + 0 + "px)";
		tagpoint.x = pospoint.x * ((rect.right - rect.left) / 600);
		tagpoint.y = pospoint.y * ((rect.bottom - rect.top) / 375);
		tagpo!.style.left = tagpoint.x + "px";
		tagpo!.style.top = tagpoint.y + "px";
		tagpo!.hidden = false;
		show_map(pos_map);
		//locate.hidden = true;
		locate!.src = "";
		maptags.push({x: tagpos.x, y: tagpos.y});
		mappoints.push({x: tagpoint.x, y: tagpoint.y});
		maps.push(pos_map);
		if (tagpos.x == 0 && tagpos.y == 0)
		{
			tagpos.x = tagpoint.x;
			tagpos.y = tagpoint.y;
		}
		let mid = {x: (tagpoint.x + tagpos.x) / 2, y: (tagpoint.y + tagpos.y) / 2};
		pointX = (rect.right - rect.left) / 2 - mid.x;
		pointY = (rect.bottom - rect.top) / 2 - mid.y - 225;
		setTransform();
		box!.style.display = "block"
		taupe!.style.display = "none";
		guess_imgs!.style.display = "none";
		bott!.style.display = "flex";
		clearInterval(chrono);
		
	}

	function change_map(name : string)
	{
		get_rect();
		tag!.hidden = true;
		tagpos = {x: 0, y: 0};
		act_map = name;
		show_map(name)
	}

	function show_map(name : string)
	{
		h_map!.hidden = true;
		w_map!.hidden = true;
		g_map!.hidden = true;
		if (name == "Hallownest")
			h_map!.hidden = false;
		if (name == "Godhome")
			g_map!.hidden = false;
		if (name == "White_palace")
			w_map!.hidden = false;
	}

	k_icon!.onmousedown = function (e) { change_map("White_palace"); select!.style.left = "136px";}
	h_icon!.onmousedown = function (e) { change_map("Hallownest"); select!.style.left = "0px";}
	g_icon!.onmousedown = function (e) { change_map("Godhome"); select!.style.left = "68px";}

	function end_scene()
	{
		scene = 2;
		scale = 1;
		pointX = 0;
		pointY = 0;
		let i = 0;

		while (i < 4)
		{
			lines.push(line.cloneNode(true));
			tags.push(tag!.cloneNode(true));
			tags[i].style.left = maptags[i].x + "px";
			tags[i].style.top = maptags[i].y + "px";
			document.getElementById("mapping")!.appendChild(tags[i]);
			points.push(tagpo!.cloneNode(true))
			points[i].style.left = mappoints[i].x + "px";
			points[i].style.top = mappoints[i].y + "px";
			document.getElementById("mapping")!.appendChild(points[i]);
			lines[i].setAttribute('x1', maptags[i].x.toString());
			
			lines[i].setAttribute('y1', maptags[i].y.toString());
			lines[i].setAttribute('x2', mappoints[i].x.toString());
			lines[i].setAttribute('y2', mappoints[i].y.toString());
			document.getElementById("box")!.appendChild(lines[i]);
			if (maptags[i].x == 0 && maptags[i].y )
			{
				tags[i].hidden = true;
				lines[i].style.display = "none";
			}
			else if (maps[i] != "Hallownest")
			{
				tags[i].hidden = true;
				points[i].hidden = true;
				lines[i].style.display = "none";
			}
			else
				tags[i].hidden = false;
			i ++;
		}
		setTransform();
		act_score!.innerHTML = "Score : " + score;
		let seconds = totalsec % 60;
		let secondsTens = Math.floor(seconds / 10);
		let secondsOnes = seconds % 10;
		let minutes = Math.floor(totalsec / 60);
		act_round!.innerHTML = "" + minutes + ":" + secondsTens + secondsOnes;
		next!.style.display = "none";
		playa!.style.display = "inline-block";
	}

	loadmap();

	place!.style.maxHeight = window.innerHeight + "px";
	place!.style.maxWidth = window.innerWidth + "px";
	rect = zoom!.getBoundingClientRect();

	window.onresize = function(event) {
		place!.style.maxHeight = window.innerHeight + "px";
		place!.style.maxWidth = window.innerWidth + "px";
		rect = zoom!.getBoundingClientRect();
	}

	function setTransform() {
		zoom!.style.transform = "translate(" + pointX + "px, " + pointY + "px) scale(" + scale + ")";
		var p = {x: pointX + tagpos.x * (scale - 1), y: pointY + tagpos.y * (scale - 1)}
		line!.setAttribute('x1', (tagpos.x + p.x).toString());
		line!.setAttribute('y1', (tagpos.y + p.y).toString());
		tag!.style.transform = "translate(" + p.x + "px, " + p.y + "px)";
		p = {x: pointX + tagpoint.x * (scale - 1), y: pointY + tagpoint.y * (scale - 1)}
		tagpo!.style.transform = "translate(" + p.x + "px, " + p.y + "px)";
		line!.setAttribute('x2', (tagpoint.x + p.x).toString());
		line!.setAttribute('y2', (tagpoint.y + p.y).toString());
		if (scene == 2)
		{
			let i = 0;
			while ( i < 4)
			{
				p = {x: pointX + maptags[i].x * (scale - 1), y: pointY + maptags[i].y * (scale - 1)};
				tags[i].style.transform = "translate(" + p.x + "px, " + p.y + "px)";
				lines[i].setAttribute('x1', (maptags[i].x + p.x).toString());
				lines[i].setAttribute('y1', (maptags[i].y + p.y).toString());
				p = {x: pointX + mappoints[i].x * (scale - 1), y: pointY + mappoints[i].y * (scale - 1)};
				points[i].style.transform = "translate(" + p.x + "px, " + p.y + "px)";
				lines[i].setAttribute('x2', (mappoints[i].x + p.x).toString());
				lines[i].setAttribute('y2', (mappoints[i].y + p.y).toString());
				i ++;
			}
		}
	}

	function StartTimer() {
		totalSeconds = 300;

		chrono = setInterval(Timer_Tick , 1000);
		let seconds = totalSeconds % 60;
		let secondsTens = Math.floor(seconds / 10);
		let secondsOnes = seconds % 10;
		let minutes = Math.floor(totalSeconds / 60);
		timer!.innerHTML = "" + minutes + ":" + secondsTens + secondsOnes;
	}

	function Timer_Tick() {
		if (totalSeconds > 0)
		{
			totalSeconds--;
			let seconds = totalSeconds % 60;
			let secondsTens = Math.floor(seconds / 10);
			let secondsOnes = seconds % 10;
			let minutes = Math.floor(totalSeconds / 60);

			timer!.innerHTML = "" + minutes + ":" + secondsTens + secondsOnes;
		}
		else
		{
			guess_scene();
		}
	}


	zoom!.onmouseleave = function(e)
	{
		panning = false;
	}

	button!.onmousedown = function(e)
	{
		let scoring
		if (tagpos.x > pospoint.x - 5 && tagpos.x < pospoint.x + 5 && tagpos.y > pospoint.y - 5 && tagpos.y < pospoint.y + 5)
			scoring = 1000;
		else
		{
			let dist = Math.sqrt((tagpos.x - pospoint.x) ** 2 + (tagpos.y - pospoint.y) ** 2) * 10;
			if (dist > 1000)
				dist = 1000;
			scoring = Math.round(1000 - dist);
		}
		act_score!.innerHTML = "Score : " + scoring;
		act_round!.innerHTML = "Round : " + Round + "/5";
		score += scoring;
		Round ++;
		guess_scene();
	}

	zoom!.onmousedown = function (e) {
		e.preventDefault();
		pos.x = (e.clientX - rect.left);
		pos.y = (e.clientY - rect.top);
		start = { x: pos.x - pointX, y: pos.y - pointY };
		point.y = pos.y;
		point.x = pos.x;
		panning = true;
	}

	zoom!.onmouseup = function (e) {
		panning = false;
		pos.x = (e.clientX - rect.left);
		pos.y = (e.clientY - rect.top);
		
		if (point.y == pos.y && point.x == pos.x && scene == 0)
		{
			tag!.hidden = false;
			tagpos.x = (pos.x - pointX) / scale;
			tagpos.y = (pos.y - pointY) / scale;
			console.log(tagpos)
			tag!.style.left = tagpos.x + "px";
			tag!.style.top = tagpos.y + "px";
			setTransform();
		}
	}

	playa!.onmousedown = function (e) {
		document.location.reload();
	}

	next!.onmousedown = function (e) {
		if (Round < 6)
			reloadmap();
		else
			end_scene();
			
	}

	zoom!.onmousemove = function (e) {
		e.preventDefault();
		if (!panning) {
		return;
		}
		pos.x = (e.clientX - rect.left);
		pos.y = (e.clientY - rect.top);
		pointX = (pos.x - start.x);
		pointY = (pos.y - start.y);
		setTransform();
	}

	zoom!.onwheel = function (e) {
		e.preventDefault();
		pos.x = (e.clientX - rect.left);
		pos.y = (e.clientY - rect.top);
		var xs = (pos.x - pointX) / scale,
		ys = (pos.y - pointY) / scale,
		delta = (e.wheelDelta ? e.wheelDelta : -e.deltaY);
		(delta > 0) ? (scale *= 1.2) : (scale /= 1.2);
		if (scale < 1)
			scale = 1;
		pointX = pos.x - xs * scale;
		pointY = pos.y - ys * scale;

		setTransform();
	}
})();
});
</script>

<style>

#zoom {
	width: 100%;
	height: 100%;
	transform-origin: 0px 0px;
	transform: scale(1) translate(0px, 0px);
	cursor: grab;
	z-index: 1;
	position: relative;
}
#tag {
	z-index: 5;
	position: absolute;
	pointer-events: none;
	overflow: visible;
	
}

div#tag > img {
	z-index: 21;
	position: relative;
	right: 25px;
	bottom: 35px;
	max-width: 50px;
}

#tagpoint {
	z-index: 3;
	position: absolute;
	pointer-events: none;
	overflow: visible;	
}

#line {
	stroke:white;
	stroke-width:2;
}

#box {
	z-index: 2;
	position: absolute;
	pointer-events: none;
	display: none;
}

#playa_text {
	display: none;
}

div#tagpoint > img {
	z-index: 20;
	position: relative;
	right: 25px;
	bottom: 25px;
	max-width: 50px;
}

.bot_ui {
	z-index: 3;
	position: absolute;
	width: 100%;
	bottom: 20px;
	height: auto;
	display:flex;
	justify-content: space-evenly;
	display: none;
	overflow: visible;
}

.top_ui {
	z-index: 3;
	position: absolute;
	width: 100%;
	top: 30px;
	/*left: 50%;*/
	display:flex;
	justify-content: space-evenly;

}

.nextimgs {
	width: auto;
	height: auto;
	position: relative;
	top: -20px;
	display: inline-block;
}

.nextimgs .img-top {
	display: none;
	position: absolute;
	top: 0;
	left: 0;
	z-index: 99;
}
.nextimgs:hover .img-top {
	display: inline;
	filter: drop-shadow(0 0 2px white)
}

div.nextimgs > img {
	filter: drop-shadow(0 0 5px black);
	width: 300px;
	height: auto;
}

span.text {
	color: white;
	font-size: 35px;
	font-family: "Trajan";
	/*text-shadow: 0 0px 10px black;*/
}

.mybutton {
	display: flex;
  	justify-content: center;
  	align-items: center;
	width: 600px;
}

.guessimgs {
	width: auto;
	height: auto;
	position: relative;
	display: inline-block;
}
.guessimgs .img-top {
	display: none;
	position: absolute;
	top: 0;
	left: 0;
	z-index: 99;
}
.guessimgs:hover .img-top {
	display: inline;
}

div.guessimgs > img {
	filter: drop-shadow(0 0 5px black);
	width: 300px;
	height: auto;
}


div#zoom > img {
	width: 100%;
	height: 100%;
}

.feature_image {
	z-index: 0;
    height: 1080px;
    width: 1920px;
	position: relative;
}

.guess_ui {
	position: absolute;
	bottom: 20px;
	right: 20px;
}

.map_ui {
	opacity: 0.5;
	background-color: white;
	width: 600px;
	height: auto;
	padding: 1px;
	transition: transform .2s;
	transform: scale(1.0);
}

.feature_image img{
	position: relative;
	height: 100%;
    width: auto;
	display: block;
	margin-left: auto;
 	margin-right: auto;
}

.zoom_outer {
	background-color: rgb(0, 0, 0);
	width: auto;
	height: auto;
}

.icon {
	width: 10%;
	height: auto;
	display: flex;
	align-items: center;
	z-index: 2;
}

.icon img{
	max-width: 100%;
	max-height: 100%;
}

.select {
	position: absolute;
	display: flex;
}

.select_back {
	height: 68px;
    background-color: #484848;
    width: 66px;
    margin: auto 0;
	mask-image: linear-gradient(to bottom, transparent, black 40%, black 60%, transparent);
}
.select_line {
	height: 68px;
    background-color: #E6E6E6;
    width: 1px;
    margin: auto 0;
	mask-image: linear-gradient(to bottom, transparent, black 45%, black 55%, transparent);
}

.icons {
	z-index: 2;
	display: flex;
}

.map_ui:hover {
	opacity: 1.0 !important;
	transform: scale(1.0);
}

</style>