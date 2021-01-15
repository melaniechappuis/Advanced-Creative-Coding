      <!DOCTYPE HTML>
      <html>
      <head>
    <script 
    src="https://nexus-js.github.io/ui/dist/NexusUI.js">
    </script>
    </head>
    <body>
    <script language="javascript" type="text/javascript">
    var con = window.Nexus.context;
    var analyser = con.createAnalyser();
    
    var inputNode; 
    
    navigator.getUserMedia = navigator.getUserMedia ||
                         navigator.webkitGetUserMedia ||
                         navigator.mozGetUserMedia;
    if (navigator.getUserMedia) {
       navigator.getUserMedia(
           { audio: true,  },
          function(stream) {
            inputNode = con.createMediaStreamSource( stream );
            inputNode.connect(analyser);
          },
          function(err) {
             console.log("The following error occurred: " + err.name);
          }
       );
    } else {
       console.log("getUserMedia not supported");
    }

    analyser.fftSize = 256;
    var bufferLength = analyser.frequencyBinCount;
    var dataArray = new Uint8Array(bufferLength);
    function setup(){
        createCanvas(600, 600);
        background(253, 140, 222);
        rectMode(CENTER);
        $('body').css('background', 'black');
    }
    var speed = 0.0;
    var angle = 0.0;
    var globalZ = 1.0;
    
    function draw(){
        noStroke();
        analyser.getByteFrequencyData(dataArray);
        translate(width/2, height/2);
        rotate(angle);

        fill(255);

        var snare = getMean(dataArray, 10, 20);  
        speed = snare / 200.0;
        globalZ = snare / 80;

        if (snare > 120.0){
            angle += (speed * 2);     
            fill(300 - snare * 2, 10022 , angle % 250);
            
        }
        else{
            angle -= 20;
            fill(250, snare * 7, 205 - snare);
        }
       
        ellipse(200, 0, (100 -snare) * globalZ, (snare - 100) * globalZ);
        ellipse(150, 150, (100 -snare) * globalZ, (snare - 100) * globalZ);
        
        
        fill(20, 20);
        rect(9, 0, width, height);
      
    }
    
    function getMean(arr, start, end){
        var total = 0.0;
        for (var i=start;i<end;i++){
            total += arr[start + i];
        }
        return total / (end-start);
    }

    setTimeout(function(){
        p5.instance = new p5();
    } , 0);
     </script>
     </body>
     </html>
