<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<title>Stylish Night/Day Calculator</title>
<meta name="viewport" content="width=device-width, initial-scale=1.0">

<style>
html, body {
  margin:0;
  padding:0;
  font-family:Arial, sans-serif;
  transition: background 0.3s, color 0.3s;
}

body.dark{
  background:#121212;
  color:#fff;
}

body.light{
  background:#f2f2f2;
  color:#000;
}

.app{
  max-width:380px;
  margin:20px auto;
  border-radius:20px;
  overflow:hidden;
  box-shadow:0 4px 15px rgba(0,0,0,0.5);
  transition: background 0.3s;
}

body.dark .app{ background:#1e1e1e; }
body.light .app{ background:#fff; }

.display{
  padding:20px;
  text-align:right;
}

.expr{
  font-size:18px;
  transition: color 0.3s;
}

body.dark .expr{ color:#bbb; }
body.light .expr{ color:#555; }

.result{
  font-size:40px;
  font-weight:bold;
  transition: color 0.3s;
}

body.dark .result{ color:#fff; }
body.light .result{ color:#000; }

.history{
  max-height:80px;
  overflow:auto;
  font-size:14px;
  transition: color 0.3s;
}

body.dark .history{ color:#888; }
body.light .history{ color:#666; }

.keys{
  display:grid;
  grid-template-columns:repeat(4,1fr);
  gap:12px;
  padding:20px;
}

button{
  height:70px;
  border:none;
  border-radius:50%;
  font-size:22px;
  transition: transform 0.1s, box-shadow 0.1s, background 0.3s, color 0.3s;
  user-select:none;
  box-shadow: 3px 3px 8px rgba(0,0,0,0.4),
              -3px -3px 8px rgba(255,255,255,0.05);
}

button:active{
  transform: scale(0.9);
  box-shadow: inset 3px 3px 8px rgba(0,0,0,0.4),
              inset -3px -3px 8px rgba(255,255,255,0.05);
}

/* Button colors */
body.dark button{ background: linear-gradient(145deg, #2c2c2c, #1e1e1e); color:#fff; }
body.light button{ background: linear-gradient(145deg, #e0e0e0, #f2f2f2); color:#000; }

.op{ background: linear-gradient(145deg, #3a3a3a, #2c2c2c); }
.equal{ background: linear-gradient(145deg, #7cbf2e, #5aa015); color:#fff; }
.ac{ color:#7cbf2e; background: linear-gradient(145deg, #3a3a3a, #2c2c2c); }

/* Toggle button small and right */
.toggle{
  display:block;
  margin:10px 20px 20px auto; /* Right aligned */
  padding:6px 10px;
  border:none;
  border-radius:50%;
  cursor:pointer;
  font-weight:bold;
  font-size:18px;
  background:#7cbf2e;
  color:#fff;
  transition: background 0.3s, color 0.3s;
}
</style>
</head>

<body class="dark">

<div class="app">
  <div class="display">
    <div class="history" id="history"></div>
    <div class="expr" id="expr"></div>
    <div class="result" id="result">0</div>
  </div>

  <div class="keys">
    <button class="ac" onclick="tap(); clr()">AC</button>
    <button class="op" onclick="tap(); cut()">⌫</button>
    <button class="op" onclick="tap(); press('%')">%</button>
    <button class="op" onclick="tap(); press('/')">÷</button>

    <button onclick="tap(); press('7')">7</button>
    <button onclick="tap(); press('8')">8</button>
    <button onclick="tap(); press('9')">9</button>
    <button class="op" onclick="tap(); press('-')">−</button>

    <button onclick="tap(); press('4')">4</button>
    <button onclick="tap(); press('5')">5</button>
    <button onclick="tap(); press('6')">6</button>
    <button class="op" onclick="tap(); press('+')">+</button>

    <button onclick="tap(); press('1')">1</button>
    <button onclick="tap(); press('2')">2</button>
    <button onclick="tap(); press('3')">3</button>
    <button class="equal" onclick="tap(); calc()">=</button>

    <button onclick="tap(); press('00')">00</button>
    <button onclick="tap(); press('0')">0</button>
    <button onclick="tap(); press('.')">.</button>
  </div>

  <!-- Toggle button below keys, small and right -->
  <button class="toggle" id="themeToggle">🌙</button>
</div>

<audio id="sound">
<source src="data:audio/wav;base64,UklGRigAAABXQVZFZm10IBAAAAABAAEAESsAACJWAAACABAAZGF0YQAAAAA=">
</audio>

<script>
let expr=document.getElementById("expr");
let result=document.getElementById("result");
let history=document.getElementById("history");
let sound=document.getElementById("sound");

function tap(){
  if(navigator.vibrate) navigator.vibrate(30);
  sound.currentTime=0;
  sound.play();
}

function press(v){
  expr.innerText+=v;
}

function clr(){
  expr.innerText="";
  result.innerText="0";
  history.innerText="";
}

function cut(){
  expr.innerText = expr.innerText.slice(0, -1);
}

function calc(){
  try{
    let e=expr.innerText.replace(/×/g,'*').replace(/÷/g,'/');
    let r=eval(e);
    history.innerHTML+=e+" = "+r+"<br>";
    result.innerText=r;
    expr.innerText="";
  }catch{
    result.innerText="Error";
  }
}

// Theme toggle with emoji
let themeBtn = document.getElementById("themeToggle");
themeBtn.addEventListener('click', ()=>{
  if(document.body.classList.contains('dark')){
    document.body.classList.remove('dark');
    document.body.classList.add('light');
    themeBtn.innerText = "🌞"; // Day emoji
  } else {
    document.body.classList.remove('light');
    document.body.classList.add('dark');
    themeBtn.innerText = "🌙"; // Night emoji
  }
});
</script>

</body>
</html>
