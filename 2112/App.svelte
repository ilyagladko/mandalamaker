<script>
	import P5 from 'p5-svelte';
  
	  var nrepeat = 7;
	  var nellps = 6;
	  var elpx = 150;
	  var elpy = 275;
	  var xinc = -20;
	  var yinc = 10;

	  let colorA = 'white';
	  let colorB = 'white';
  
	  var pwidth = window.innerWidth;
	  var pheight = window.innerHeight;
	  var xcen = pwidth*0.5;
	  var ycen = pheight*0.5;
  
	  let opacity = 0.9;
	//   var bgcolor = "black"
	  var bgcolor = "black";
	  var weight = 1;
	//   let mix = 0;
	//   let buttonColor;
	//   let buttonRandom;
	  
	  
	  // let colorA = 'white'
	  // let colorB = 'blue'
	  
	  function getRandomColor() {
		  var letters = '0123456789ABCDEF';
		  var color = '#';
		  for (var i = 0; i < 6; i++) {
			  color += letters[Math.floor(Math.random() * 16)];
			}
			return color;
		}


			const randomize = () => {
				nrepeat = Math.random()*8;
				nellps = Math.random()*10;
				elpx = Math.random()*200;
				elpy = Math.random()*200;
				xinc = Math.random()*(300-100);
				yinc = Math.random()*(300-100);
				opacity = Math.random()*1;
				weight = (Math.random()*1)+0.2;

				// colorA = p5.getRandomColor();
				// colorB = p5.getRandomColor();

				// reColor();
			}

		const sketch = (p5) => {

			// let div = p5.createDiv('Hello ').size(100, 20);
			// div.html('World', true);
			
			const reColor = () => {
				colorA = p5.color(getRandomColor());
				colorB = p5.color(getRandomColor());
				
				console.log(colorA+" "+colorB);
				}


				
			p5.setup = () => {
				let c = p5.createCanvas(pwidth, pheight);
				p5.smooth();
				
				// buttonColor = p5.createButton('recolor');
				// buttonColor.position(20, 20);
  				// buttonColor.mousePressed(reColor);

				// buttonRandom = p5.createButton('random');
				// buttonRandom.position(100, 20);
  				// buttonRandom.mousePressed(randomize);

		  p5.background(bgcolor);
	  };
  
	//   colorA = p5.color(getRandomColor());
	  colorA = p5.color('white');
	  colorB = p5.color(getRandomColor());

	  p5.draw = () => { 
			p5.noStroke();  
			p5.fill(bgcolor); 
			p5.rect(0,0,pwidth,pheight);

			// let mixTarget = p5.map(p5.mouseX, 0, pwidth, 0.0, 1.0);
	  		// mix = mix + ((mixTarget - mix));
			// console.log(mix);

		let angrot = 0.0;
		let angrotinc = 180.0/nrepeat;
  
		  let melpy = elpy;
			if (yinc > 0) { melpy = elpy + (nellps * yinc); }
	  
		  let mscale = (pheight) / melpy;
  
		  p5.noFill();
		  p5.strokeWeight(weight);
		//  p5.stroke('rgba(255,255,255,'+(opacity)+')');

		  for (let j = 0; j < nrepeat; j++) {
			  ellps((elpx*mscale),(elpy*mscale),nellps,(xinc*mscale),(yinc*mscale),angrot)
			  angrot = angrot + angrotinc;
		  }
		  
		function ellps (radx,rady,num,radxinc,radyinc,ellprot ) {
			p5.colorMode('RGB')
			p5.angleMode(p5.DEGREES); 
			let nradx = radx;
			let nrady = rady;
			// const colorA = p5.color('red');
			// const colorB = p5.color('#344231');
			// console.log (colorA);
			// const colorB = Math.floor(Math.random()*16777215).toString(16);
  
			  for (let i = 0; i < num; i++) {
				  p5.stroke(p5.lerpColor(colorA,colorB,(i/num)));
				//   p5.stroke('white');
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


function saveImage (){
  var link = document.createElement('a');
  link.download = 'mandala'+Math.round(Math.random()*9999)+'.png';
  link.href = document.getElementById('defaultCanvas0').toDataURL()
  link.click();
}

//   function savePNG (p5) {
//   	//let pngname = str(nf(month(),2))+str(nf(day(),2))+str(year())+"_"+str(nf(hour(),2))+str(nf(minute(),2))+str(nf(second(),2));

// 	  photo = loadImage('assets/rockies.jpg');
// }

// function draw() {
//   p5.image(photo, 0, 0);
// }

// function keyTyped() {
//   if (key === 's') {
//     p5photo.save('photo', 'png');
//   }
// }

//   	let newfile = "Mandala";
//   	let img = p5.get(0,0,pwidth,pheight);
//   	p5.save(newfile, 'png');
//   	setTimeout(4000);
//   }
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
	  <!-- <div class="row">
		  <p>Opacity {opacity}</p>
			  <input type="range" class="slider" bind:value={opacity} min="0.1" max="1" step="0.05" />
	  </div> -->
	  <div class="row">
		  <p>Weight {weight}</p>
			  <input type="range" class="slider" bind:value={weight} min="0.1" max="4" step="0.05" />
	  </div>
	  <div class="row">
		<button class="butt" on:click={saveImage}>save</button>
		<button class="butt" on:click={randomize}>random</button>
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
	  .container {
		  display: flex;
		  align-items: center;
		  justify-content: center;
  }
  .controls {
	/* display: none; */
	  position: fixed;
	  display: flex;
	  flex-direction: column;
	  justify-content: space-evenly;
	  /* align-items: end; */
	  /* right: 20px; */
	  /* bottom: 10px; */
	  left: 20px;
	  z-index: 9;
	  height: calc(100vh - 80px);
	}
	.row {
		  /* width: calc(100vw-40px); */
		  display: flex;
		  flex-direction: row;
		  /* height: 100px; */
		  /* width: calc(100vw - 40px); */
		  align-items: center;
		  justify-content: right;
		  /* gap: 54px; */
	  }
	  .butt {
		font-size: 12px;
		line-height: 20px;
		background:transparent;
		color: white;
		border: 0;
		/* text-color */
	  }
	  p {
		  color: rgba(255,255,255,0.6);
		  font-size: 12px;
		  line-height: 20px;
		  display: none;
	  }
	  input[type="range"] {
		  -webkit-appearance: none;
		  -moz-appearance: none;
		  appearance: none;
		  border: 0;
		  background: transparent;
		  /* background: black; */
		  /* width: 250px; */
		  width: calc(100vw - 40px);
		  height: calc((100vh - 40px)/8);
		  /* outline: 0; */
		  cursor: grab;
	  }
  
	  input[type="range"]::-webkit-slider-thumb {
		  -webkit-appearance: none;
		  appearance: none;
		  /* background-color: rgba(255,255,255,0.8); */
		  background-color: white;
		  /* background-color: transparent; */
		  /* border-color: white; */
		  width: 8px;
		  height: 8px;
		  border-radius: 50%;
		  cursor: grab;
		  transition: .3s ease-out;
		}
		
		input[type="range"]::-webkit-slider-thumb:hover {
			background-color: rgba(255,255,255,1);
			transform: scale(1.3);
		}
	  
	  input[type="range"]::-webkit-slider-thumb:active {
	  transform: scale(1.4);
	}

	input[type=range]::-ms-track {
    width: 80px;
    height: 80px;
    
    /*remove bg colour from the track, we'll use ms-fill-lower and ms-fill-upper instead */
    background: transparent;
    
    /*leave room for the larger thumb to overflow with a transparent border */
    border-color: transparent;
    border-width: 6px 0;

    /*remove default tick marks*/
    color: transparent;
}
  
  @media only screen and (max-width: 1000px) {
	  .row {
		  /* height: 100px; */
		  gap: 16px;
	  }
	  p {
		display: none;
	  }
	  .controls {
		width: 100%;
		  z-index: 9;
	  }
	  /* input[type="range"] {
		  -webkit-appearance: none;
		  -moz-appearance: none;
		  appearance: none;
		  border: 0;
		  background: transparent;
		  height: 100px;
		  width: 600px;
		  outline: 0;
		  cursor: pointer;
	  } 
	  */
  
	  input[type="range"]::-webkit-slider-thumb {
		  /* width: 4px; */
		  /* height: 4px; */
		  border-radius: 50%;
		  cursor: grab;
		  transition: .3s ease-out;
	  }
  }
  </style>