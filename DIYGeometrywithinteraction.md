

     <html>

     <body>
	<div
	
  
             style="background:magenta; height:805px;"
		           onmousemove="con_amp.gain.value=5*(1**(event.clientX/180)) ;osc.frequency.value = 165 * (2 ** ((event.clientY-5)/50) );"
	>

	</div>
       </body>
       <style>
       
 This is to make sure
 the canvas is in the right position
 on all browsers    
 

         canvas {
         position: absolute;
         top:100;
         left:250;
         }
  
    </style>

    <canvas></canvas>
    <script>

    var fov = 300;

    var canvas = document.querySelector("canvas");
    var width = 400;
    var height = 400;
    var context = canvas.getContext("2d");
    canvas.setAttribute("width", width);
    canvas.setAttribute("height", height);
    canvas.addEventListener('mousemove',getMouse,false);
    
    var mouseX=0;
    var mouseY=0;

     var point = [];
     var points = [];
     var point3d = [];

    var angleX = 2;
    var angleY = 0;
    var HALF_WIDTH = width / 2;
    var HALF_HEIGHT = height / 2;

    var x3d = 0;
    var y3d = 0;
    var z3d = 0;

    var counter=0;

    var lastx2d = 0;
    var lasty2d = 0;
    var dim = 50; 
    var spacing = ((Math.PI * 90) / dim);
    var numPoints = dim * dim;
    var size = 200;
      
    var audio_context = window.AudioContext || window.webkitAudioContext;
      
    var con = new audio_context();
      
    var con_amp = con.createGain();
    con_amp.gain.value = 0.00001 ;
  
   create an oscillator
	     
	 var osc = con.createOscillator();
	  
   connect the oscillator to the amplification
      
       osc.connect(con_amp);
      
   connect the oscillator to the audio output
	
	con_amp.connect(con.destination);
      
   set the frequency
	
	osc.frequency.value = 440;
      
  start the oscillator
    
	osc.start();

     function draw() {

     counter=0.01; 

    var points = [];

    for (var i = 0; i < numPoints; i++) {

     var z = size * Math.cos(spacing * i); 
  
     var s = size * Math.cos(spacing * i);
        
     point = [Math.cos(spacing * i * mouseX) * Math.cos(spacing * i * counter) * s,Math.sin(spacing * i * mouseY) * Math.sin(spacing * i * counter) * s,z];
        points.push(point);
        
    
    }

    context.globalAlpha = "0.02";
    context.fillStyle = "rgb(255,0,255)";
    context.fillRect(1, 0, width, height);
    
    angleX+=((mouseX/width)-0.5)/2;
    angleY+=((mouseY/height)-0.5)/4;

Here we run through each point and work out where it should be drawn

     for (var i = 0; i < numPoints; i++) {
        point3d = points[i];
        z3d = point3d[2];
        

Check if the points are too close
if (z3d < -fov) z3d += HALF_WIDTH;
        
        point3d[2] = z3d;
 
 Calculate the rotation using our own functions
     
    rotateX(point3d,angleX);
    rotateY(point3d,angleY);
 
 Get the point in position 
        
	x3d = point3d[0];
        y3d = point3d[1];
        z3d = point3d[2];

Convert the Z value to a scale factor
This will give the appearance of depth
        
	var scale = fov / (fov + z3d);

Store the X value with the scaling
FOV is taken into account

        var x2d = (x3d * scale) + HALF_WIDTH;

Store the Y value with the scaling
FOV is taken into account

        var y2d = (y3d * scale) + HALF_HEIGHT;

Draw the point

Set the size based on scaling
        
	if (scale>20) scale =20;
        context.lineWidth = scale;
        context.strokeStyle = "rgb(255,255,0)";
        context.beginPath();
        context.moveTo(x2d-100, y2d);
        context.lineTo(lastx2d-100, lasty2d);
        context.stroke();
        
        lastx2d=x2d;
        lasty2d=y2d;
    }

    }

    setInterval(draw, 50);

Our own rotate functions

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

 here's our function 'getMouse'.

     function getMouse (mousePosition) {
for other browsers..
   
   
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

     </html>
