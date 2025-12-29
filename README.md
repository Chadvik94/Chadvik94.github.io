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
const unlockTime = new Date(2026,0,13,0,0,0);
const lock = document.getElementById("lockScreen");
const timer = document.getElementById("timer");
const camera = document.getElementById("cameraScreen");
const next = document.getElementById("next");
const slides = ["s1","s2","s3","s4"];
let i = 0;

/* FORCE LOCK SCREEN */
lock.classList.add("active");

/* INIT */
checkState();

/* Decide state */
function checkState(){
  const now = new Date();
  if(now >= unlockTime){
    startCamera();
  }else{
    updateTimer();          // 👈 render immediately
    setInterval(updateTimer, 1000);
  }
}

/* TIMER */
function updateTimer(){
  const diff = unlockTime - new Date();
  if(diff <= 0){
    location.reload();
    return;
  }
  const h = String(Math.floor(diff/3600000)).padStart(2,"0");
  const m = String(Math.floor((diff%3600000)/60000)).padStart(2,"0");
  const s = String(Math.floor((diff%60000)/1000)).padStart(2,"0");
  timer.innerText = `⏳ ${h}:${m}:${s}`;
}

/* CAMERA */
function startCamera(){
  lock.classList.remove("active");
  camera.classList.add("active");
  let c = 3;
  const countEl = document.getElementById("count");
  const t = setInterval(()=>{
    c--;
    countEl.innerText = c;
    if(c === 0){
      clearInterval(t);
      camera.classList.remove("active");
      showSlide(0);
      next.style.display = "block";
    }
  },1000);
}

/* SLIDES */
function showSlide(n){
  slides.forEach(id=>document.getElementById(id).classList.remove("active"));
  document.getElementById(slides[n]).classList.add("active");
  i = n;
}

next.onclick = ()=>{
  if(i < slides.length-1){
    showSlide(i+1);
  }else{
    next.style.display="none";
    hearts();
  }
};

/* HEARTS */
function hearts(){
  setInterval(()=>{
    const h = document.createElement("div");
    h.className="heart";
    h.innerText="💗";
    h.style.left=Math.random()*100+"vw";
    document.body.appendChild(h);
    setTimeout(()=>h.remove(),3000);
  },180);
}
</script>
