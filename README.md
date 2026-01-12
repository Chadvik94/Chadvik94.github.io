<html lang="en">
<head>
<meta charset="UTF-8">
<title>For Sanika 🥰</title>

<link href="https://fonts.googleapis.com/css2?family=Manufacturing+Consent&display=swap" rel="stylesheet">

<style>
*{margin:0;padding:0;box-sizing:border-box;}

body{
  background:#f4efe6;
  font-family:'Manufacturing Consent', cursive;
  overflow:hidden;
}

/* COMMON */
.screen{
  display:none;
  height:100vh;
  width:100%;
  justify-content:center;
  align-items:center;
  flex-direction:column;
  text-align:center;
}
.screen.active{display:flex;}

/* CAMERA */
.camera-frame{
  border:6px solid #222;
  border-radius:20px;
  padding:60px 80px;
  background:#fffaf3;
  position:relative;
}
.record-dot{
  position:absolute;
  top:15px;
  left:15px;
  width:14px;
  height:14px;
  background:red;
  border-radius:50%;
  animation:blink 1s infinite;
}
@keyframes blink{50%{opacity:.3}}

#countdown{
  font-size:96px;
  color:#d86a7a;
}

/* SLIDES */
.slide img{
  max-width:80%;
  max-height:80vh;
  border-radius:16px;
  box-shadow:0 20px 40px rgba(0,0,0,0.2);
}

/* NEXT BUTTON */
.next-btn{
  position:fixed;
  right:30px;
  bottom:30px;
  font-size:30px;
  background:#d86a7a;
  color:white;
  border:none;
  border-radius:50%;
  width:60px;
  height:60px;
  cursor:pointer;
  display:none;
}

/* HEART RAIN */
.heart{
  position:fixed;
  top:-20px;
  font-size:24px;
  animation:fall 3s linear forwards;
}
@keyframes fall{
  to{transform:translateY(110vh);opacity:0}
}
</style>
</head>

<body>

<!-- 📸 CAMERA -->
<div id="cameraScreen" class="screen active">
  <div class="camera-frame">
    <div class="record-dot"></div>
    <div id="countdown">3</div>
  </div>
</div>

<!-- SLIDES -->
<div id="slide1" class="screen slide"><img src="1.png"></div>
<div id="slide2" class="screen slide"><img src="2.png"></div>
<div id="slide3" class="screen slide"><img src="3.png"></div>
<div id="slide4" class="screen slide"><img src="4.png"></div>

<button id="nextBtn" class="next-btn">➜</button>

<script>
const slides = ["slide1","slide2","slide3","slide4"];
let index = 0;
const nextBtn = document.getElementById("nextBtn");
const countdownEl = document.getElementById("countdown");

/* CAMERA COUNTDOWN (iPhone-safe) */
let count = 3;
function cameraCountdown(){
  countdownEl.textContent = count;
  if(count > 1){
    count--;
    setTimeout(cameraCountdown,1000);
  }else{
    setTimeout(()=>{
      document.getElementById("cameraScreen").classList.remove("active");
      showSlide(0);
      nextBtn.style.display = "block";
    },1000);
  }
}
cameraCountdown();

/* SHOW SLIDE */
function showSlide(n){
  slides.forEach(id=>{
    document.getElementById(id).classList.remove("active");
  });
  document.getElementById(slides[n]).classList.add("active");
  index = n;
}

/* NEXT BUTTON */
nextBtn.onclick = ()=>{
  if(index < slides.length - 1){
    showSlide(index + 1);
  }else{
    nextBtn.style.display = "none";
    startHearts();
  }
};

/* HEART RAIN */
function startHearts(){
  setInterval(()=>{
    const h = document.createElement("div");
    h.className = "heart";
    h.textContent = "💗";
    h.style.left = Math.random()*100 + "vw";
    document.body.appendChild(h);
    setTimeout(()=>h.remove(),3000);
  },150);
}
</script>

</body>
</html>
