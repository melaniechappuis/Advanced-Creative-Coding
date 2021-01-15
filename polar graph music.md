     <html>

     <head>
     <script src = "https://mimicproject.com/libs/maximilian.js"></script>

     </head>

    <body>
    <canvas></canvas>

    </body>
    <script type="text/javascript">

     var changeThis = 50;
     var maximJs = maximilian();
     var maxiAudio = new maximJs.maxiAudio();
  
  create a bunch of stuff
                                     
     var sample = new maximJs.maxiSample();
     maxiAudio.init();
     var osc = new maximJs.maxiOsc();
     var osc2 = new maximJs.maxiOsc();
     var osc3 = new maximJs.maxiOsc();
    
     maxiAudio.loadSample("finaleager.mp3", sample);
    
     var drawOutput = new Array(1024);
     var counter = 0;

    var canvas = document.querySelector("canvas");
    var width = window.innerWidth;
    var height = window.innerHeight;
    var context = canvas.getContext("2d");
    canvas.setAttribute("width", width);
    canvas.setAttribute("height", height);

    var spacing = ((Math.PI * 2) / 1050);
    var size = 1000;
  	var position = 300;
	
  This works out a frequency we can use that matches the buffersize
             
	var bufferFreq= 44100/1024;

        maxiAudio.play = function() {
   var wave = (osc.sawn(bufferFreq) - osc2.sawn(bufferFreq*1.001));
   var wave = osc.sinewave(bufferFreq+osc2.sinewave(bufferFreq*changeThis)*osc3.sinewave(0.01)*1000);
   var wave = osc.sinewave(bufferFreq*osc2.sinewave(bufferFreq)*10) * 1.15;
                     
      var wave = sample.play();
      counter++;
      
      drawOutput[counter % 1024] = wave;
      return wave * 1;
     };

     var colors = ["red",
             "blue",
              "yellow",
             ]


    function draw() {

    context.clearRect(0, 0, width, height);

     for (var i = 0; i < 1024; i++) {
	context.beginPath();

	context.moveTo(position + (Math.cos(i * spacing) * size * drawOutput[i]),(height /2) + (Math.sin(i * spacing) * size * drawOutput[i]));

	context.lineTo(position + (Math.cos(i * spacing) * drawOutput[i]),(height /2) + (Math.sin(i * spacing) * drawOutput[i]));
  
     color_idx = i%10;
     context.strokeStyle = colors[color_idx];
	context.stroke();
	context.closePath();
    document.body.style.backgroundColor ="#f000ff";

    
    }
    requestAnimationFrame(draw);
    }

     requestAnimationFrame(draw);
     </script>

     </html>
