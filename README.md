<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<title>Store Exterior Lights</title>

<style>
*{margin:0;padding:0;box-sizing:border-box;}

body{
  height:100vh;
  background:#020202;
  display:flex;
  justify-content:center;
  align-items:flex-end;
  overflow:hidden;
  font-family:Arial, sans-serif;
}

/* Night sky */
.sky{
  position:absolute;
  inset:0;
  background:linear-gradient(#020202,#060606 60%, #0b0b0b);
}

/* Light spread */
.light-glow{
  position:absolute;
  inset:0;
  background:
    radial-gradient(circle at 30% 45%, rgba(255,230,150,0.45), transparent 55%),
    radial-gradient(circle at 50% 45%, rgba(255,230,150,0.45), transparent 55%),
    radial-gradient(circle at 70% 45%, rgba(255,230,150,0.45), transparent 55%);
  opacity:0;
  transition:0.6s;
  pointer-events:none;
}
.light-glow.on{opacity:1;}

/* Store front */
.store{
  position:relative;
  width:100%;
  height:55%;
  background:#111;
  border-top:4px solid #222;
}

/* Store sign */
.sign{
  position:absolute;
  top:40px;
  left:50%;
  transform:translateX(-50%);
  width:320px;
  height:60px;
  background:#1a1a1a;
  border-radius:8px;
}

/* Bulb area */
.bulbs{
  position:absolute;
  top:110px;
  width:100%;
  display:flex;
  justify-content:space-around;
}

/* Cord */
.cord{
  width:4px;
  height:90px;
  background:#aaa;
  margin:auto;
}

/* Bulb */
.bulb{
  width:70px;
  height:70px;
  background:#222;
  border-radius:50%;
  margin:auto;
  transform-origin:top center;
}

.bulb.on{
  background:#ffe08a;
  box-shadow:0 0 25px #ffd54f, 0 0 60px #fff2b0;
}

.bulb::after{
  content:"";
  position:absolute;
  inset:35%;
  border-radius:50%;
  background:#ffb300;
  opacity:0;
}
.bulb.on::after{opacity:1;}

/* Swing animation */
.swing{
  animation:swing 2.2s ease-in-out infinite alternate;
}
@keyframes swing{
  from{transform:rotate(-4deg);}
  to{transform:rotate(4deg);}
}

/* Switch */
.switch-box{
  position:absolute;
  right:40px;
  bottom:80px;
  width:60px;
  height:100px;
  background:#ddd;
  border-radius:8px;
  display:flex;
  justify-content:center;
  align-items:center;
}

.switch{
  width:26px;
  height:54px;
  background:#666;
  border-radius:5px;
  cursor:pointer;
  transition:0.3s;
}

.switch.on{
  background:#222;
  transform:translateY(12px);
}
</style>
</head>

<body>

<div class="sky"></div>
<div class="light-glow" id="glow"></div>

<div class="store">
  <div class="sign"></div>

  <div class="bulbs">
    <div>
      <div class="cord"></div>
      <div class="bulb" id="b1"></div>
    </div>
    <div>
      <div class="cord"></div>
      <div class="bulb" id="b2"></div>
    </div>
    <div>
      <div class="cord"></div>
      <div class="bulb" id="b3"></div>
    </div>
  </div>

  <div class="switch-box">
    <div class="switch" id="switch"></div>
  </div>
</div>

<!-- Sounds -->
<audio id="click" src="https://assets.mixkit.co/sfx/preview/mixkit-light-switch-click-2583.mp3"></audio>
<audio id="hum" src="https://assets.mixkit.co/sfx/preview/mixkit-electricity-hum-1950.mp3" loop></audio>

<script>
let on=false;
const bulbs=[b1,b2,b3];
const sw=document.getElementById("switch");
const glow=document.getElementById("glow");
const click=document.getElementById("click");
const hum=document.getElementById("hum");

function flicker(b){
  let i=0;
  const f=setInterval(()=>{
    b.classList.toggle("on");
    i++;
    if(i>6){
      clearInterval(f);
      b.classList.add("on");
    }
  },80);
}

sw.onclick=()=>{
  on=!on;
  click.play();

  if(on){
    sw.classList.add("on");
    glow.classList.add("on");
    hum.play();

    bulbs.forEach((b,i)=>{
      setTimeout(()=>{
        flicker(b);
        b.classList.add("swing");
      },i*140);
    });

  }else{
    sw.classList.remove("on");
    glow.classList.remove("on");
    hum.pause(); hum.currentTime=0;

    bulbs.forEach(b=>{
      b.classList.remove("on","swing");
    });
  }
};
</script>

</body>
</html>
  
