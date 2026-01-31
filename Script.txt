const canvas = document.getElementById("canvas");
const ctx = canvas.getContext("2d");
const imgUpload = document.getElementById("imgUpload");

let history = [];

function saveState(){
  history.push(canvas.toDataURL());
}

function undo(){
  if(history.length > 0){
    let img = new Image();
    img.src = history.pop();
    img.onload = () => ctx.drawImage(img,0,0);
  }
}

function addRect(){
  saveState();
  ctx.fillStyle = "blue";
  ctx.fillRect(50,50,200,120);
}

function addCircle(){
  saveState();
  ctx.beginPath();
  ctx.arc(350,150,60,0,Math.PI*2);
  ctx.fillStyle="red";
  ctx.fill();
}

function addText(){
  saveState();
  let t = prompt("Enter Text");
  ctx.font = "30px Arial";
  ctx.fillStyle = "black";
  ctx.fillText(t,100,300);
}

imgUpload.addEventListener("change", function(e){
  saveState();
  let file = e.target.files[0];
  let img = new Image();
  img.src = URL.createObjectURL(file);
  img.onload = () => ctx.drawImage(img,200,200,250,180);
});

function clearCanvas(){
  ctx.clearRect(0,0,canvas.width,canvas.height);
}
