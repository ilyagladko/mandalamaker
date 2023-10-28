<script>
  import P5 from 'p5-svelte';

	var nrepeat = 7;
	var nellps = 6;
	var elpx = 150;
	var elpy = 275;
	var xinc = -20;
	var yinc = 10;

	var pwidth = 720;
	var pheight = 720;
	var xcen = (pheight/2);
	var ycen = (pwidth/2);

	let opacity = 0.9;
	var bgcolor = "black"
	var weight = 1;

	const sketch = (p5) => {
		p5.setup = () => {
		p5.smooth();

		p5.createCanvas(pwidth, pheight);
		p5.background(bgcolor);
	};

	p5.draw = () => { 
  		p5.noStroke();  
  		p5.fill(bgcolor); 
  		p5.rect(0,0,pwidth,pheight);

		let angrot = 0.0;
		let angrotinc = 180.0/nrepeat;

		let melpy = elpy;
  		if (yinc > 0) { melpy = elpy + (nellps * yinc); }
	
		let mscale = (pheight-25) / melpy;

		p5.noFill();
		p5.strokeWeight(weight);
		p5.stroke('rgba(255,255,255,'+(opacity)+')');
		
		for (let j = 0; j < nrepeat; j++) {
			ellps((elpx*mscale),(elpy*mscale),nellps,(xinc*mscale),(yinc*mscale),angrot)
    		angrot = angrot + angrotinc;
    	}
		
		function ellps (radx,rady,num,radxinc,radyinc,ellprot ) {
			p5.angleMode(p5.DEGREES); 
			let nradx = radx;
			let nrady = rady;
	
			for (let i = 0; i < num; i++) {
				p5.push();
				p5.translate(xcen,ycen);
				p5.rotate(ellprot);
				p5.ellipse(0,0,nradx,nrady);
		
				p5.pop();

				nradx = nradx + radxinc;
				nrady = nrady + radyinc;
			}
		}
	}
}
// const savePNG = (p5) => {
// 	//let pngname = str(nf(month(),2))+str(nf(day(),2))+str(year())+"_"+str(nf(hour(),2))+str(nf(minute(),2))+str(nf(second(),2));
// 	let newfile =  "Mandala";
// 	// let img = p5.get(0,0,pwidth,pheight);
// 	p5.save(newfile, 'png');
// 	setTimeout(4000);
// }
</script>

<div class="controls">
	<div class="row">
		<p>Ellipses {nrepeat}</p>
			<input type="range" bind:value={nrepeat} min="1" max="50" step="1" />
	</div>
	<div class="row">
		<p>Repeat {nellps}</p>
			<input type="range" bind:value={nellps} min="1" max="20" step="1" />
	</div>
	<div class="row">
		<p>Radius X {elpx}</p>
			<input type="range" class="slider" bind:value={elpx} min="100" max="600" step="1" />
	</div>
	<div class="row">
		<p>Radius Y {elpy}</p>
			<input type="range" class="slider" bind:value={elpy} min="1" max="300" step="1" />
	</div>
	<div class="row">
		<p>Indent X {xinc}</p>
			<input type="range" class="slider" bind:value={xinc} min="-100" max="300" step="1" />
	</div>
	<div class="row">
		<p>Indent Y {yinc}</p>
			<input type="range" class="slider" bind:value={yinc} min="-100" max="300" step="1" />
	</div>
	<div class="row">
		<p>Opacity {opacity}</p>
			<input type="range" class="slider" bind:value={opacity} min="0.1" max="1" step="0.05" />
	</div>
	<div class="row">
		<p>Weight {weight}</p>
			<input type="range" class="slider" bind:value={weight} min="0.1" max="4" step="0.05" />
	</div>
	<!-- <div class="row">
		<button on:click={savePNG}>Save PNG</button>
	</div> -->
</div>

<div class="container">
	<P5 {sketch} />
</div>

<style>
	:global(html, body) {
		margin: 0;
		padding: 0;
		width: 100%;
		height: 100%;
		background-color: black;
		-webkit-font-smoothing: antialiased;
	}
	.container {
		display: flex;
		align-items: center;
		justify-content: center;
}
	.row {
		display: flex;
		flex-direction: row;
		height: 26px;
		width: 100%;
		align-items: center;
		justify-content: right;
		gap: 14px;
	}
	.controls {
		position: fixed;
		display: flex;
		flex-direction: column;
		align-items: end;
		right: 20px;
		bottom: 10px;
		z-index: 9;
	}
	p {
		color: white;
		font-size: 12px;
		line-height: 20px;
	}
	input[type="range"] {
		-webkit-appearance: none;
		-moz-appearance: none;
		appearance: none;
		border: 0;
		background: transparent;
		width: 250px;
		height: 24px;
		outline: 0;
		cursor: pointer;
	}

	input[type="range"]::-webkit-slider-thumb {
		-webkit-appearance: none;
		appearance: none;
		background-color: white;
		width: 8px;
		height: 8px;
		border-radius: 50%;
		cursor: grab;
		transition: .3s ease-out;
	}

	input[type="range"]::-webkit-slider-thumb:hover {
    	transform: scale(1.3);
  	}
	
	input[type="range"]::-webkit-slider-thumb:active {
    transform: scale(1.4);
  }

@media only screen and (max-width: 700px) {
	.row {
		height: 28px;
		gap: 20px;
	}
	.controls {
		z-index: 9;
	}
	input[type="range"] {
		-webkit-appearance: none;
		-moz-appearance: none;
		appearance: none;
		border: 0;
		background: transparent;
		height: 40px;
		width: 260px;
		outline: 0;
		cursor: pointer;
	}

	input[type="range"]::-webkit-slider-thumb {
		-webkit-appearance: none;
		appearance: none;
		background-color: white;
		width: 16px;
		height: 16px;
		border-radius: 50%;
		cursor: grab;
		transition: .3s ease-out;
	}
}
</style>