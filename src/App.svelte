<script>
	import P5 from 'p5-svelte';
	import { saveAs } from 'file-saver';
	import Range from "./Range.svelte"	
	
	let c = "";
	let flow = true;

	// let h1 = 0;
	// let s1 = 130;
	// let b1 = 130;
	let h1 = Math.ceil(Math.random()*255);
	let s1 = Math.ceil(Math.random()*255);
	let b1 = Math.ceil(Math.random()*55)+200;
	
	let h2 = Math.ceil(Math.random()*255);
	let s2 = Math.ceil(Math.random()*255);
	let b2 = Math.ceil(Math.random()*55)+200;

	let colorSpeed = 10;
	let sizeSpeed = 20;
	let sizeAmp = 30;

	// let h2 = 0;
	// let s2 = 50;
	// let b2 = 130;

	let wWidth = window.innerWidth;
	let wHeight = window.innerHeight;
	let bgcolor = "black";	
	 let sizeTime = 0;
	 let colorTime = 0;

	let quantity = Math.ceil(Math.random()*10)+2;
	let ellipses = Math.ceil(Math.random()*8)+1;
	let ellipseX = 150;
	let ellipseY = 275;
	let offsetX = -20;
	let offsetY = 10;
	let w = 15;
	$: weight = w/10;
	let flowLabel = "flow on";

	function onFlow() {
		flow = !flow;
		flowLabel = flow ? "flow on" : "flow off";
		console.log(flow)
	}
	
	const sketch = (p5) => {
		p5.setup = () => {
			c = p5.createCanvas(wWidth, wHeight);
			p5.colorMode(p5.HSB, 255);
			p5.smooth();		
			
			p5.background(bgcolor);
			console.log(p5.colorMode);

			console.log(h1, s1, b1);
			
			let col1 = p5.color(h1, s1, b1);
			let col2 = p5.color(h2, s2, b2);

		p5.draw = () => {
			if (flow) {

				sizeTime += sizeSpeed * sizeSpeed * 0.00001;
				colorTime += colorSpeed * colorSpeed * 0.001;
			}
			if (sizeTime > 255) { sizeTime -= 255; }
			if (colorTime > 255) { colorTime -= 255; }

		p5.noStroke();  
		p5.fill(bgcolor);
		p5.rect(0,0,wWidth,wHeight);

		let sineX = Math.sin(sizeTime) * sizeAmp * 4;
		let cosY = Math.cos(sizeTime) * sizeAmp * 4;
		//ellipseX = sineX;
		//ellipseY += cosY;

		let angrot = 0.0;
		let angrotinc = 180.0/quantity;
  
		let mellipseY = ellipseY;
		if (offsetY > 0) { mellipseY = ellipseY + (ellipses * offsetY); }
	  
		let mscale = (wHeight) / mellipseY;
  
		p5.noFill();
		p5.strokeWeight(weight);

		col1 = p5.color(h1, s1, b1);
		col2 = p5.color(h2, s2, b2);
		
		// if (h1 = 255 || h1 > 255) {
		// 	h1 -= 255;
		// }

		
		// h1 ++;
		// if (h1 >= 255) h1 = 0
		// h2 ++;
		// if (h2 >= 255) h2 = 0

	//	console.log(h1);
		
		for (let j = 0; j < quantity; j++) {
		// 	if(!flow){
		// 		ellps((ellipseX*mscale),(ellipseY*mscale),ellipses,(offsetX*mscale),(offsetY*mscale),angrot)
		// 	}
		// else {

			ellps(((ellipseX + sineX)*mscale),((ellipseY + cosY)*mscale),ellipses,(offsetX*mscale),(offsetY*mscale),angrot)
		// }
			// let add = (j/quantity) + sizeTime * 10;
			// ellps(((ellipseX + sineX + Math.sin(add) * amp)*mscale),((ellipseY + cosY + Math.cos(add) * amp)*mscale),ellipses,(offsetX*mscale),(offsetY*mscale),angrot)
   
			  angrot = angrot + angrotinc;
		}

		function ellps (radx,rady,num,radoffsetX,radoffsetY,ellprot ) {
			p5.angleMode(p5.DEGREES); 
			let nradx = radx;
			let nrady = rady;

			  for (let i = 0; i < num; i++) {
				  var finColor = p5.lerpColor(col1,col2,(i/num));
				  /*
				  // let lerpHue = 
				  */
			// if ( flow ) {

					 let he = p5.hue(finColor);
				//	 if(flow){
						 he += colorTime;
					// }
					 if(he > 255)
					 {
						 he -= 255;
						}
						var finfinColor = p5.color(he,p5.saturation(finColor),p5.brightness(finColor));
						p5.stroke(finfinColor);
				//	 }
				//	 else {
				//		p5.stroke(finColor);
				//	}
				
						p5.push();
						p5.translate(wWidth*0.5,wHeight*0.5);
						p5.rotate(ellprot);
						p5.ellipse(0,0,nradx,nrady);
		  
						p5.pop();
						
						nradx = nradx + radoffsetX;
				  nrady = nrady + radoffsetY;
				}
				
		  }
	  }

    }
}

	function saveImage (){
		// var link = document.createElement('a');
		// link.download = 'mandala'+Math.round(Math.random()*9999)+'.png';
		// link.href = document.getElementById('defaultCanvas0').toDataURL()
		// link.click();

		// const dataUrl = document.getElementById('defaultCanvas0').toDataURL("png");
  		// console.log(dataUrl);
  		// const win = window.open(dataUrl, '_blank');
		// window.open(document.getElementById('defaultCanvas0').toDataURL("image/png"), '_blank');

		var canvas = document.getElementById("defaultCanvas0");
			canvas.toBlob(function(blob) {
    		saveAs(blob, 'mandala'+Math.round(Math.random()*9999)+'.png');
		});

	}

	function randomize () {
		quantity = Math.ceil(Math.random()*10)+2;
		ellipses = Math.ceil(Math.random()*8)+1;
		ellipseX = Math.ceil(Math.random()*200);
		ellipseY = Math.ceil(Math.random()*200);
		offsetX = Math.ceil((Math.random()*300)-100);
		offsetY = Math.ceil((Math.random()*300)-100);
		weight = (Math.random()*2)+0.5;

		h1 = Math.ceil(Math.random()*256);
		s1 = Math.ceil(Math.random()*156)+100;
		b1 = Math.ceil(Math.random()*156)+100;

		h2 = Math.ceil(Math.random()*256);
		s2 = Math.ceil(Math.random()*156)+100;
		b2 = Math.ceil(Math.random()*156)+100;
	}


</script>
	
<div class="controls">
	<div class="row">
		<Range bind:value={quantity} min={1} max={30} />
	</div>
	<div class="row">
		<Range bind:value={ellipses} min={1} max={25} />
	</div>
	<div class="row">
		<Range bind:value={ellipseX} min={50} max={500} />
	</div>
	<div class="row">
		<Range bind:value={ellipseY} min={1} max={300} />
	</div>
	<div class="row">
		<Range bind:value={offsetX} min={-100} max={300} />
	</div>
	<div class="row">
		<Range bind:value={offsetY} min={-100} max={300} />
	</div>
	<div class="row">
		<Range bind:value={w} min={5} max={30} />
	</div>
	<div class="row">
		<Range bind:value={h2} min={1} max={255} />
		<Range bind:value={s2} min={1} max={254} />
		<Range bind:value={b2} min={1} max={254} />
	</div>
	<div class="row">
		<Range bind:value={h1} min={1} max={255} />
		<Range bind:value={s1} min={1} max={254} />
		<Range bind:value={b1} min={1} max={254} />
	</div>
	<div class="row">
		<Range bind:value={colorSpeed} min={0} max={100} />
		<Range bind:value={sizeSpeed} min={0} max={100} />
		<Range bind:value={sizeAmp} min={0} max={100} />
	</div>
	<div class="butts">
		<button class="butt" on:click={saveImage}>save</button>
		<button class="butt" on:click={randomize}>random</button>
		<!-- <button class="butt" on:click={onFlow}>{flowLabel}</button> -->
	</div>
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
	.controls {
		display: flex;
		position: fixed;
		flex-direction: column;
		justify-content: stretch;
		top: 0;
		bottom: 0;
		left: 0;
		right: 0;
		z-index: 99;
	}
	
	.row {
		display: flex;
		flex-direction: row;
		height: 100%;
	}
	
	.container {
		position: absolute;
		top: 0;
		bottom: 0;
		left: 0;
		right: 0;
		display: flex;
		align-items: center;
		justify-content: center;
		z-index: 0;
	}

	.butts {
		padding-left: 24px;
		padding-bottom: 24px;
	}

	.butt {
		font-size: 12px;
		line-height: 20px;
		background:transparent;
		color: white;
		border: 0;
	  }
</style>