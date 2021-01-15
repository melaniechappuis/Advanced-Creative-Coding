<html>

<head>
 <script src = "https://mimicproject.com/libs/maximilian.js"></script>

</head>

<body>
    <canvas></canvas>

</body>
<script type="text/javascript">
	
	//This is where we are going to store the mouse information
	
   var changeThis = 1;
   var maximJs = maximilian();
   var maxiAudio = new maximJs.maxiAudio();
   var mouseX;
   var mouseY;
   
   maxiAudio.init();
    
   var osc = new maximJs.maxiOsc();
   var osc2 = new maximJs.maxiOsc();
   var osc3 = new maximJs.maxiOsc();
   var samp = new maximJs.maxiSample();
   var output;
   var drawOutput = new Array(1);
   var counter = 0;

   var canvas = document.querySelector("canvas");
   
     //This sets the width and height to the document window
     
   var width = window.innerWidth;
   var height = window.innerHeight;
   
   
    //Be aware that when you resize the window, you will need to call (do) this again
  
    //This creates a 2d drawing 'context' in your canvas
  
   var context = canvas.getContext("2d");
   canvas.setAttribute("width", width);
   canvas.setAttribute("height", height);
   
    //We really need this 
    
   var spacing = ((Math.PI * 1300) / 552);
   var size = width / 2;
  	
  canvas.addEventListener('mousemove', getMouse, false);
  
  function getMouse(mousePosition) {
    
   if (mousePosition.layerX || 
        mousePosition.layerX === 0) {  
        mouseX = mousePosition.layerX;
        mouseY = mousePosition.layerY;
    } else if (mousePosition.offsetX ||
              mousePosition.offsetX === 0) {
      mouseX = mousePosition.offsetX;
      mouseY = mousePosition.offsetY;
      
   }
    
  }
  
  
    // This works out a frequency we can use that matches the buffersize
    //var bufferFreq=44100/512;
    
    
  maxiAudio.play = function() {
        
  var wave = Math.abs(osc.sawn(81) * osc2.square(97));
  
       //var wave = (1 + osc.sawn(bufferFreq) - osc2.sawn(bufferFreq * 1.01));
       //var wave = osc.sinewave(bufferFreq+osc2.sinewave(100+bufferFreq*changeThis)*osc3.sinewave(0.01)*900);
       //var wave = osc.sinewave(bufferFreq*osc2.sinewave(20+bufferFreq)*10) * 2.5;
       
       
 counter++;
      
   drawOutput[counter % 512] = wave;
   return wave;
};
  
  var segments = mouseX/10;
  var segmentsy= mouseY/10;
  
  var colors = ["red",
             "blue",
              "yellow",
             ]

function draw() {

   var segments = mouseX/250;
   var segmentsy = mouseY/250;
  context.clearRect(0, 0, width, height);

for (var i = 0; i < 556; i++) {
	context.beginPath();
  
 color_idx = i%10;
 context.strokeStyle = colors[color_idx];
  

 context.moveTo((width / segments) + (Math.cos(i * spacing) * size * drawOutput[i]),(height / segmentsy) + (Math.sin(i * spacing) * size * drawOutput[i]));

context.lineTo((width / segments) + (Math.cos(i * spacing) * drawOutput[i]),(height / segmentsy) + (Math.sin(i * spacing) * drawOutput[i]));
  
context.stroke();
context.closePath();
  document.body.style.backgroundColor ="#0000ff";
        }
  requestAnimationFrame(draw);
    }

requestAnimationFrame(draw);
  </script>

</html>       
       
       
       
       
       
       
     
