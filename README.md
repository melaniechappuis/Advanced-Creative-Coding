 <html>

<head>
   <script src = "https://luuma.net/lib/maximilian.js"></script>
</head>

<style>

/*  
    This is to make sure
    the canvas is in the right position
    on all browsers    
*/

canvas {
position: absolute;
top:0;
left:0;
}
  
  body {
    
   background-color: #ffffff; 
    
  }

</style>

<body>
    <canvas></canvas>
    <script>
    var maxi = maximilian();
	var maxiEngine = new maxi.maxiAudio();
    maxiEngine.init();
	var kick = new maxi.maxiSample();
	var hihat= new maxi.maxiSample();
  	var hihat1 = new maxi.maxiSample();
    var hihat2 = new maxi.maxiSample();
    var snare = new maxi.maxiSample();
	var kick2 = new maxi.maxiSample();
    var clap = new maxi.maxiSample(); 
	var sample2 = new maxi.maxiSample();
    var sample3 = new maxi.maxiSample();
    var sample4 = new maxi.maxiSample();
    var myOsc = new maxi.maxiOsc();
    var myOsc2 = new maxi.maxiOsc();
    var myOsc3 = new maxi.maxiOsc();
    var filter = new maxi.maxiFilter();
	var filter2 = new maxi.maxiFilter();
 	var filter3 = new maxi.maxiFilter();
    var env1 = new maxi.maxiEnv();
    var env2 = new maxi.maxiEnv();
    var env3 = new maxi.maxiEnv();
    var tempo = 220;  
      var draw1 = false;
      var draw2 = false;
      var draw3 = false;
 	env1.setAttack(10);
  	env2.setAttack(20);
  	env1.setDecay(1000);
  	env2.setDecay(1000);
  	env3.setDecay(2000);
  	env1.setSustain(500);
  	env1.setRelease(75);
    var colour = "blue";
    var musicX = 0;
    var musicY = 0;
    var drawOutput1 = new Array(104);
    var drawOutput2 = new Array(1524);
    var drawOutput3 = new Array(1024);
    var drawOutput4 = new Array(1024);
    var drawOutput5 = new Array(1024);
      
      
      var prob1 = 0.1;
      var prob2 = 3.5;
  //var envOut = env1.adsr(1, env1.adsr.trigger);
	var clock = new maxi.maxiClock();
	
  clock.setTempo(tempo);
  clock.setTicksPerBeat(1);
	
      var drawInterval = 100;
                  
var fov = 300;

var canvas = document.querySelector("canvas");
var width = window.innerWidth*1.2;
var height = window.innerHeight*1.2;
var context = canvas.getContext("2d");
canvas.setAttribute("width", width);
canvas.setAttribute("height", height);
canvas.addEventListener('mousemove',getMouse,false);
var mouseX=3;
var mouseY=3;
var barCounter =0;
var point = [];
var points = [];
var point3d = [];

var angleX = 0;
var angleY = 0;
var HALF_WIDTH = width / 2;
var HALF_HEIGHT = height / 2;

var x3d = 0;
var y3d = 0;
var z3d = 0;

var counter=0;
var counterAudio = 0;
var lastx2d = 0;
var lasty2d = 0;
var dim = 50; 
var spacing = ((Math.PI * 10) / dim);
var numPoints = dim * dim;
var size = 80;

      
      
      
      
       function playAudio() {
	
  	 maxiEngine.loadSample("kick.wav", kick);
     maxiEngine.loadSample("hihat2.wav", hihat1);
     maxiEngine.loadSample("snare.wav", snare);
     maxiEngine.loadSample("clap", clap); 
     maxiEngine.loadSample("leap.wav", sample2);
     maxiEngine.loadSample("sample.wav", sample3);
     
	 

		  maxiEngine.play = function() {
		  var wave = 100;
            
          counterAudio++;
      	  clock.ticker();
            
   
          
      if (clock.isTick() ) 
      {
        let beatCounter = clock.playHead % 3;
          		
        console.log(beatCounter);
        
               if (beatCounter > 9)
                {
              		barCounter+= 1;
                  dim = 50;
                  if (barCounter == 30){
                    barCounter = 1;
                  }
                  
            	}
        
             console.log(barCounter);
        
        //kick trigger
          if (Math.random() > prob2 && [0, 3, 5, 9].indexOf(beatCounter)&7 != 0) 
          {
            
            kick.trigger();
            musicX = Math.sin(100*barCounter) * mouseX;
            musicY = Math.sin(110*barCounter) * mouseY;
            drawInterval = 50;
            colour = "rgba(0,0,225)";
            prob1 = 0.1;
          }
        
        //kick2 (aka snare) trigger
         if (Math.random() > 0.75 && beatCounter  > 7) 
          {
          
          	snare.trigger();	
            musicX = Math.cos(musicY)*(mouseY/50)^2 * Math.sin(musicX)*(mouseX/50)^2;
            drawInterval = 90;
            
            if (barCounter%2 ==0) 
            {
                 colour = "rgba(255,255,0)";
            }
            else if (barCounter%2 == 1)
            {
              colour = "rgba(225,255,225)";
            }
            prob1 = 0.01;
          }
        
         if (Math.random() > 0.5 && beatCounter%1 == 0 )
         	{
              kick2.trigger();
              musicY = Math.sin(musicX);
              colour = "rgba(250,30,220)";
                //let hihatSpeed = ((clock.playHead % 11 / 7) + 0.5) * -1;
              prob1 = 0.7;

         	}
         if (Math.random() > 0.8  && [5, 7, 11].indexOf(beatCounter)%13 != 0)
         	{   
              sample3.trigger();
              musicX = 100;
              musicY= Math.cos(mouseY*170)*mouseX;
              colour = "rgba(255,255,0)";
              dim = 30;
              drawInterval = 150;
              if (Math.random() > 0.8){
                sample4.trigger();
                draw3 = true;
                draw1 = false;
                draw2 = false;
                prob1 = 1.8;
              }
         	}
     
        
        if (Math.random() > 0.7 )
              {
                let hihatSpeed = ((clock.playHead % 11 / 7) + 0.5) * -1;
                colour = "rgba(102,255,105)";
           
                if (Math.random() > 0.9)
                 {
                  clap.trigger();
                  fov -= 5;
                   dim = 10;
                   draw2 = false;
                   draw1 = true;
                   draw3 = false;
                   prob2 = 0.3;
                 }               
              }
              
      
          if (Math.random() > 0.35 && [0,1,2,4,5,6].indexOf(beatCounter)&7 >= 0) 
          {
                hihat2.trigger();
            sample4.trigger();
            if(Math.random() > 0.7)
              {
              fov = 100+Math.random();
              }
          }
        
        //hi hat 1 and sample 2
        if (Math.random() > 0.5 && [3,4,5,7, 9].indexOf(beatCounter)&11 >= 0) {
                hihat1.trigger();
          if (Math.random() > prob1) 
          {
            sample2.trigger();
            colour = "rgba(255,204,255)";
            fov -=5;
            if (draw1){
              sample4.trigger();
              prob2 = 0.4;
            }
          }
               
       }

        //hithat 2
        if (beatCounter % 5 == 2 && Math.random() > prob1)
        {
          hihat2.trigger();
          colour = "rgba(255,255,0)";
          musicX = Math.cos(beatCounter^2 + barCounter^2);
          musicY = Math.sin(musicX^beatCounter);
         draw1 = false;
          draw2 = true;
          draw3 = false;
          prob2 = 0.75
          if (Math.random > 0.9){
            sample4.trigger();
          }
        }
        
        
 
        
      }     
            
            if (Math.random() > 0.9)
            {
              let hihatSpeed = (clock.playHead % 11 / 5) * myOsc.sinewave(1000) + 0.5 ;
              let kickSpeed = 0.3;
            }
            
            
            else hihatSpeed = (clock.playHead % 11 / 5) + 0.5; 
            kickSpeed =  Math.random(); 
            
      let snareSpeed = 0.3*clock.playHead%3;
          //Math.sin((clock.playHead % 17) * (Math.random() * 0.2)  + 0.5, Math.random() * 3 )% Math.max((clock.playHead % 19) * (Math.random() * 0.05)  + 0.5, Math.random() * 3 ); 
      let sample2Speed =  Math.sign((clock.playHead % 1 / 5) * myOsc.sinewave(1 * myOsc.coswave(0.3)) + 0.5) * Math.random(); 
      let sample3Speed = (clock.playHead % 1); 
      let sample2Amp =  myOsc.sinewave(0.3 * clock.playHead ) * 1.2; 
      let clapSpeed = ((clock.playHead % 1) ) - (mouseX/10)*0.1; 
      let clapAmp = clock.playHead % 6 ? 0.2 : 0.7; 
      let filter1Cutoff = Math.pow(((myOsc2.saw(0.143) + 1) /2), 2) * 20 + 40;
      let filter2Cutoff = myOsc.sinewave(200) * 20;
            
       
            
      var wave1 = kick.playOnce(kickSpeed+(0.3)) + hihat2.playOnce(0.4+snareSpeed) * myOsc.sinewave(0.4);
            
      var wave2 = filter.hires(sample2.playOnce(sample2Speed*(mouseY/100)) * sample2Amp, filter1Cutoff , 3) + filter2.lores(kick2.playOnce(1),  filter2Cutoff , 3);
            
      //w += sample2.playOnce(sample2Speed) * sample2Amp;
      var wave3 = snare.playOnce(snareSpeed/2) + clap.playOnce(clapSpeed) * clapAmp;
            
      var wave4 = sample3.playOnce(sample3Speed) * 2;// + sample4.playOnce(0.7);
            
      let hihatAmp = clock.playHead % 2 ? 0.3 : 1;
      var wave5 = hihat.playOnce(hihatSpeed) * hihatAmp + hihat1.playOnce(hihatSpeed);
            var wave6 = sample4.playOnce(snareSpeed/2)  + hihat1.playOnce(hihatSpeed) * hihatAmp;
      
            wave = (wave1 + wave2 + wave3 + wave4 + wave5 + wave6);
            
            drawOutput1[counterAudio % 1024] = wave2*(mouseY/100);
            drawOutput2[counterAudio % 1023] = wave1*(mouseX/100);
            drawOutput3[counterAudio % 1023] = wave3;
            drawOutput4[counterAudio % 1023] = wave4;
            drawOutput5[counterAudio % 1023] = (wave1 * wave2 * wave3 * wave4 * wave5);// /(5*Math.sin(mouseX)
            
            return wave;
          }
	}
    
      
      
  playAudio();    
      
      
function draw() {

counter=0.01; 

var points = [];

    for (var i = 0; i < numPoints; i++) {
	var q = i % 1024;
     var z = size * Math.sin(spacing * i); 
  
     var s = size * Math.cos(spacing * i);
     
      if (drawOutput1[q] > drawOutput2[q])
      {
        if(Math.random > prob1){
          point = [i*size/dim,j*size/dim*drawOutput5[q], Math.sin((wave2 * mouseY * drawOutput4[q]) + (wave * mouseX * drawOutput3[q]))* 10.0];
        }
        
        
        else {   point = [Math.tan(spacing * i + drawOutput2[q]) * Math.sin(spacing * i * (counter/4) + drawOutput3[q]) * s,Math.sin(spacing * i * musicY) * Math.sin(spacing * i * counter) * s ,z + drawOutput2[q]];
//point = [Math.cos(spacing * i * musicY- (Math.sin(drawOutput1[q]- drawOutput1[q-7] ))) * Math.sin(spacing * i + drawOutput2[q]) * s,Math.sin(spacing * musicX * i + Math.sin(drawOutput1[q] / drawOutput2[q])) * Math.sin(spacing * i * counter - drawOutput5[q]) * s,z];
        }
    }
  
      else if (draw2){
         point = [Math.cos(spacing * (i * Math.sin(z))+ drawOutput2[q]) * Math.sin(spacing * i * counter + drawOutput5[q]) * s,Math.sin(spacing * i * musicY) * Math.sin(spacing * i * (counter/2)) * s,z];
        //point = [Math.tan(spacing * i * musicY- (Math.cos(drawOutput3[q]- drawOutput5[q-7] ))) * Math.abs(spacing * i + drawOutput2[q]) * s *drawOutput3[q*drawOutput5[q-barCounter*7]],Math.sin(spacing * musicX * barCounter*(mouseY/100) + Math.cos(drawOutput4[q] % drawOutput3[q])) * Math.sin(spacing * i * counter - drawOutput5[q]) * s,z];
      }
      else if (draw1)
      {
            
         point = [Math.cos(spacing * i * musicX) * Math.sin(spacing * i * counter) * s,Math.sin(spacing * i * musicY) * Math.sin(spacing * i * counter) * s,z];
            }
        
       
        
      else if (draw3){
               point = [Math.tan(spacing * i * musicX) * Math.cos(spacing * i * counter) * s,Math.sin(spacing * i * musicY) * Math.sin(spacing * i * counter) * (s*Math.cos(z)) ,z];
      }
      
      /*
     else if (draw3){
     point = [Math.cos(spacing * i * musicX) * Math.sin(spacing * i * counter) * s,Math.sin(spacing * i * musicY) * Math.sin(spacing / i * counter) * (s*Math.cos(z*drawOutput[counter%1024])) ,z];
      }
      */
      else {
        point = [Math.cos(spacing * i * musicX- (Math.sin(drawOutput1[q]* drawOutput1[q] ))) * Math.cos(spacing * i + drawOutput2[q]) * s,Math.sin(spacing * (barCounter%13) * i + Math.tan(drawOutput1[q] / drawOutput2[q])) * Math.sin(spacing * i * counter - drawOutput5[q]) * s + mouseX,z-musicY];
        
      }
        points.push(point);
        
    
}

    context.globalAlpha = "0.1";
    context.fillStyle = 'rgba('+ i*drawOutput5[counter % 1025]*(mouseY/100) + ','+ i*drawOutput3[counter % 1025] + ','+ i*drawOutput4[counter % 1025] + ', '+ i*drawOutput1[counter % 1025]*(mouseX/100)- ' )';
    context.fillRect(255, 255, 0, width, height);
    
    angleX+=((musicX/width)-0.5)/(barCounter%23+1);
    angleY+=((musicY/height)-0.5)/(barCounter%13+2);

// Here we run through each point and work out where it should be drawn

    for (var i = 0; i < numPoints; i++) {
        point3d = points[i];
        z3d = point3d[2];
        

// Check if the points are too close
//        if (z3d < -fov) z3d += HALF_WIDTH;
        
        point3d[2] = z3d;
 
 // Calculate the rotation using our own functions
    rotateX(point3d,angleX);
    rotateY(point3d,angleY);
 
 // Get the point in position 
        x3d = point3d[0];
        y3d = point3d[1];
        z3d = point3d[2];
// Convert the Z value to a scale factor
// This will give the appearance of depth
        var scale = fov / (fov + z3d);

// Store the X value with the scaling
// FOV is taken into account

        var x2d = (x3d * scale) + HALF_WIDTH;

// Store the Y value with the scaling
// FOV is taken into account

        var y2d = (y3d * scale) + HALF_HEIGHT;

// Draw the point

// Set the size based on scaling
        if (scale>20) scale =20;
      
      if (draw1){
        context.lineWidth = scale+ drawOutput5[Math.floor(Math.random()*(barCounter^musicX))] ;
        context.strokeStyle = colour;
        context.beginPath();
        context.moveTo(x2d-musicX, y2d + musicY);
        context.lineTo(lastx2d+musicY, lasty2d);
        context.stroke();
      }
      else if (draw2){
        context.lineWidth = scale / drawOutput3[Math.floor(Math.random()*(barCounter^musicX))] ;
        context.strokeStyle = colour;
        context.beginPath();
        context.moveTo(x2d-musicX, y2d + musicY);
        context.lineTo(lastx2d+musicY, lasty2d);
        context.stroke();
      }
      else if(draw3){
        context.lineWidth = scale - drawOutput5[Math.floor(Math.random()*(barCounter^musicY))] ;
        context.strokeStyle = colour;
        context.beginPath();
        context.moveTo(x2d, y2d - musicY);
        context.lineTo(lastx2d+musicY, lasty2d);
        context.stroke();
      }
        lastx2d=x2d;
        lasty2d=y2d;
    }

}

setInterval(draw, drawInterval);
      

// Our own rotate functions

function rotateX(point3d,angleX) {
        var	x = point3d[0]; 
        var	z = point3d[2]; 
	
        var	cosRY = Math.cos(angleX);
        var	sinRY = Math.sin(angleX);
        
        var	tempz = z; 
        var	tempx = x;

        x= (tempx*cosRY)+(tempz*sinRY);
        z= (tempx*-sinRY)+(tempz*cosRY);

        point3d[0] = x;
        point3d[2] = z;
          
}

function rotateY(point3d,angleY) {
        var y = point3d[1];
        var	z = point3d[2]; 
	
        var cosRX = Math.cos(angleY);
        var sinRX = Math.sin(angleY);
        
        var	tempz = z; 
        var	tempy = y;

        y= (tempy*cosRX)+(tempz*sinRX);
        z= (tempy*-sinRX)+(tempz*cosRX);

        point3d[1] = y;
        point3d[2] = z;
          
} 

//here's our function 'getMouse'.
function getMouse (mousePosition) {
//for other browsers..
  if (mousePosition.layerX || mousePosition.layerX === 0) { // Firefox?
    mouseX = mousePosition.layerX;
    mouseY = mousePosition.layerY;
  } else if (mousePosition.offsetX || mousePosition.offsetX === 0) { // Opera?
    mouseX = mousePosition.offsetX;
    mouseY = mousePosition.offsetY;
  }
}

var lastOut=0;

function lowPassFilter(input,coeff){
    var output = lastOut+coeff*(input-lastOut);
    lastOut=output;
    return output;
}
   
</script>

</body>

</html>
