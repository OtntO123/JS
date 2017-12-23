<html>
<style>
#container {
  width: 400;
  height: 400;
  position: relative;
  background: yellow;
}
#animate {
  width: 40;
  height: 40;
  position: absolute;
  background-color: red;
}
#ballanima {
  width: 40;
  height: 40;
  position: absolute;
  background-color: green;
}
</style>
<body ONKEYPRESS = "myFunction(event)">

<p id="demo"></p>
<div id ="container">
<div id ="ballanima"></div>
<div id ="animate"></div>
</div>


<script>
var v = -40;
var h = 0;
var containerElement = document.getElementById("container");

var initiateAnima =  document.getElementById("animate");
initiateAnima.style.left = "160";
initiateAnima.style.top = "160";

var initiateBall =  document.getElementById("ballanima");
initiateBall.style.left = "160";
initiateBall.style.top = "160";

Array.prototype.insert = function ( index, item ) {
    this.splice( index, 0, item );
};



document.getElementById("demo").innerHTML = "Hungry Snake! Press Click Me to Strat. Use W S A D to Control!";

function myFunction() {
  var x = event.which;
    
  if (x == 119) {
    v = -40;
    h = 0;
  } else if (x == 115) {
    v = 40;
    h = 0;
  } else if (x == 97) {
    h = -40;
    v = 0;
  } else if (x == 100) {
    h = 40;
    v = 0;
  }
  
//document.getElementById("demo").innerHTML = "X: " + h + "  Y: " + v;
}
</script>

<p>
<button onclick="myMove()" id="button1">Click Me</button>
</p> 


<script>

var elem = document.getElementById("animate");

var ballForEat = document.getElementById("ballanima");

function myMove() {
  document.getElementById('button1').style.visibility = 'hidden';
  var movestep = 1;
  var posx = 160;
  var posy = 160;
  var speed = 1000;
  var snakeBody = [];

  var ball = random2DimInt();    
  ballForEat.style.left = ball[0] * 40 + 'px';
  ballForEat.style.top = ball[1] * 40 + 'px';


  var id = setInterval(frame, speed);

  function frame() {
//pushDivObject(objt);
//pushDivObject(objt);

//changeStyleLeft(objt[0], 1);
//changeStyleTop(objt[0], 2);

//deleteFirstElementOfArray(snakeBody);
//getFirstElementOfArray(snakeBody)[0];

    if (movestep % 3 == 0) {
      clearInterval(id);
      speed = 9000 / (movestep + 10) + 100;
      id = setInterval(frame, speed);
    }
    
    posx = posx + h;
    posy = posy + v;

    if (posx >= 400 || posx < 0 || posy >= 400 || posy < 0 || checkHeadinSnakebody()) {
	  bang();
      return 0;
    }

    if (posx/40 == ball[0] && posy/40 == ball[1]) {
      pushDivObject(snakeBody);
      lastDiv = snakeBody[0];
      lastDiv.style.left = posx - h;
      lastDiv.style.top = posy - v;

      ball = random2DimInt();  
      ballForEat.style.left = ball[0] * 40;
      ballForEat.style.top = ball[1] * 40;

    } else if(snakeBody.length >=1) {
      ThePointOfTail = snakeBody[snakeBody.length - 1];
      ThePointOfTail.style.left = posx - h;
      ThePointOfTail.style.top = posy - v;
      snakeBody.insert(0, ThePointOfTail);
      snakeBody.splice(snakeBody.length - 1 ,snakeBody.length);
    }
    
    


    
    
    elem.style.left = posx + 'px';
    elem.style.top = posy + 'px';
    movestep++;
  }
  
  function getFirstElementOfArray(array) {
	return array[Object.keys(array)[0]];
  }

  function deleteFirstElementOfArray(array) {
    delete array[Object.keys(array)[0]];
  }
  
  function bang() {
    clearInterval(id);
    document.getElementById("demo").innerHTML = "Bang! Merry Christmas! Your Score is " + snakeBody.length + ".";
  }
  
  function random2DimInt() {
    var max = 10;
    var min = 0;
    var bonus;
    do {
      bonus = [Math.floor(Math.random() * (max - min)) + min, Math.floor(Math.random() * (max - min)) + min];
/*
      if (snakeBody.length>=1) {
        ppoint = snakeBody[0];
        document.getElementById("demo").innerHTML = point.style.left;
      }
*/
    } while ( checkarrayinarray(bonus, snakeBody) );
    return bonus;
  }

  function checkarrayinarray(arr0, arr) {
    arrvolumn = arr.length;
    for (i = 0; i < arrvolumn; i++) {
      point = arr[i];
      positionX = getRemovedPXstring(point.style.left);
      positionY = getRemovedPXstring(point.style.top);
      if(positionX == arr0[0] && positionY == arr0[1]) {	//Wrong!Does not RUN! Got IT!
      	return true;
      }
    }
    if (posx == arr0[0] && posy == arr0[1]) {
      return true;
    }
    return false;
  }
  
  
 function checkHeadinSnakebody() {
   for (i = 0, snakeBodyVolumn = snakeBody.length; i < snakeBodyVolumn; i++) {
     point = snakeBody[i];
     positionX = getRemovedPXstring(point.style.left) * 40;
     positionY = getRemovedPXstring(point.style.top) * 40;
//   document.getElementById("demo").innerHTML = positionX/40 + " " + posx/40 + " " + positionY/40 + " " + posy/40 + " " + snakeBody.length;
     if(positionX == posx && positionY == posy) {	//Wrong!Does not RUN! Got IT!
       return true;
     }
   }
   
   
   return false;
 }
  
  function getRemovedPXstring(str) {
    string = parseInt(str.substring(0, str.length-2)) / 40;
    return string;
  }

  function pushDivObject(obj) {
    AddBody = document.createElement("div");
    AddBody.id = 'BodyObject';
    AddBody.style.width = "40";
    AddBody.style.height = "40";
    AddBody.style.position = "absolute";
    AddBody.style.background = "red";
    AddBody.style.left = "160";
    AddBody.style.top = "160";
    containerElement.appendChild(AddBody);
    obj.insert(0, AddBody);
  }

  function changeStyleLeft(obj, len) {
    var objt = obj;
    fixedLen = parseInt(len) * 40;
    objt.style.left = fixedLen;
  }

  function changeStyleTop(obj, len) {
    var objt = obj;
    fixedLen = parseInt(len) * 40;
    objt.style.top = fixedLen;
  }
}
</script>

</body>
</html>

