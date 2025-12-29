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

/* LOCK SCREEN */
#lockScreen{
  font-size:26px;
  color:#555;
}
#timer{
  margin-top:18px;
  font-size:20px;
  color:#d86a7a;
}

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

<!-- 🔒 LOCK -->
<div id="lockScreen" class="screen">
  🔒 This surprise unlocks on<br>
  <b>13 January 2026</b>
  <div id="timer"></div>
</div>

<!-- 📸 CAMERA -->
<div id="cameraScreen" class="screen">
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
/* ========= CONFIG ========= */
const UNLOCK_TIME = new Date(2026,0,13,0,0,0);
/* ========================= */

const lockScreen = document.getElementById("lockScreen");
const cameraScreen = document.getElementById("cameraScreen");
const timerEl = document.getElementById("timer");
const nextBtn = document.getElementById("nextBtn");
const slides = ["slide1","slide2","slide3","slide4"];
let currentSlide = 0;
let unlocked = false;
let timerInterval = null;

/* Decide initial state */
function init(){
  const now = new Date();
  if(now >= UNLOCK_TIME){
    startExperience();
  }else{
    lockScreen.classList.add("active");
    startTimer();
  }
}

/* Countdown timer only (no unlock logic loop) */
function startTimer(){
  timerInterval = setInterval(()=>{
    const now = new Date();
    const diff = UNLOCK_TIME - now;
    if(diff <= 0){
      clearInterval(timerInterval);
      location.reload(); // SAFEST for iPhone
      return;
    }
    const h = String(Math.floor(diff/3600000)).padStart(2,"0");
    const m = String(Math.floor((diff%3600000)/60000)).padStart(2,"0");
    const s = String(Math.floor((diff%60000)/1000)).padStart(2,"0");
    timerEl.innerText = `⏳ ${h}:${m}:${s}`;
  },1000);
}

/* Start main flow */
function startExperience(){
  if(unlocked) return;
  unlocked = true;
  lockScreen.classList.remove("active");
  cameraScreen.classList.add("active");
  startCamera();
}

/* Camera */
function startCamera(){
  let c = 3;
  const el = document.getElementById("countdown");
  const camTimer = setInterval(()=>{
    c--;
    if(c>0){
      el.innerText=c;
    }else{
      clearInterval(camTimer);
      cameraScreen.classList.remove("active");
      showSlide(0);
      nextBtn.style.display="block";
    }
  },1000);
}

/* Slides */
function showSlide(i){
  slides.forEach(id=>document.getElementById(id).classList.remove("active"));
  document.getElementById(slides[i]).classList.add("active");
  currentSlide = i;
}

nextBtn.onclick = ()=>{
  if(currentSlide < slides.length-1){
    showSlide(currentSlide+1);
  }else{
    nextBtn.style.display="none";
    startHearts();
  }
};

/* Hearts */
function startHearts(){
  const heartInterval = setInterval(()=>{
    const h=document.createElement("div");
    h.className="heart";
    h.innerText="💗";
    h.style.left=Math.random()*100+"vw";
    document.body.appendChild(h);
    setTimeout(()=>h.remove(),3000);
  },180);
}

/* INIT */
init();
</script>

</body>
</html>
