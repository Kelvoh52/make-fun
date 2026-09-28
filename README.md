<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1, viewport-fit=cover">
<title>Will you go out with me?</title>
<link href="https://fonts.googleapis.com/css2?family=Bricolage+Grotesque:opsz,wght@12..96,400;12..96,700;12..96,800&display=swap" rel="stylesheet">
<style>
:root{--bg:#fde8ef;--ink:#3b1030;--soft:#7a4a6b;--card:#fff;--pink:#e8336d;--pink-ink:#fff;--line:#f2b8cd}
@media (prefers-color-scheme:dark){:root{--bg:#2a0d24;--ink:#ffe8f1;--soft:#d9a3bf;--card:#3d1633;--pink:#ff5c93;--pink-ink:#2a0d24;--line:#6b2c58}}
*{box-sizing:border-box}
body{margin:0;min-height:100vh;background:var(--bg);color:var(--ink);font-family:"Bricolage Grotesque",system-ui,sans-serif;display:flex;align-items:center;justify-content:center;padding:20px;overflow-x:hidden}
main{width:100%;max-width:460px;text-align:center}
.step{display:none}.step.on{display:block;animation:pop .35s ease}
@keyframes pop{from{transform:scale(.96);opacity:0}to{transform:none;opacity:1}}
h1{font-size:clamp(2rem,9vw,3rem);line-height:1.05;margin:.2em 0 .3em;font-weight:800;letter-spacing:-.02em}
p{color:var(--soft);font-size:1.05rem;line-height:1.5;margin:0 0 1.4em}
.big{font-size:5rem;line-height:1;margin-bottom:.1em}
.row{display:flex;gap:12px;justify-content:center;align-items:center;min-height:64px;position:relative}
button,input{font:inherit}
.btn{background:var(--pink);color:var(--pink-ink);border:0;border-radius:999px;padding:14px 30px;font-weight:700;font-size:1.1rem;cursor:pointer}
.btn.ghost{background:transparent;color:var(--ink);border:2px solid var(--line)}
.btn:disabled{opacity:.4;cursor:not-allowed}
.grid{display:grid;grid-template-columns:1fr 1fr;gap:10px;margin-bottom:1.4em}
.opt{background:var(--card);color:var(--ink);border:2px solid var(--line);border-radius:18px;padding:16px 8px;cursor:pointer;font-weight:700}
.opt span{display:block;font-size:2rem;margin-bottom:4px}
.opt[aria-pressed="true"]{border-color:var(--pink);background:var(--pink);color:var(--pink-ink)}
input{width:100%;background:var(--card);color:var(--ink);border:2px solid var(--line);border-radius:16px;padding:14px;margin-bottom:12px}
.sum{background:var(--card);border:2px solid var(--line);border-radius:22px;padding:8px 20px;text-align:left;margin-bottom:1.4em}
.sum div{display:flex;justify-content:space-between;gap:12px;padding:12px 0;border-bottom:1px solid var(--line)}
.sum div:last-child{border:0}.sum b{color:var(--soft);font-weight:400}
#no{transition:left .12s,top .12s;touch-action:none}
#no.run{position:fixed;z-index:9;margin:0}
.dots{display:flex;gap:6px;justify-content:center;margin-bottom:22px}
.dots i{width:24px;height:5px;border-radius:9px;background:var(--line)}.dots i.f{background:var(--pink)}
</style>
</head>
<body>
<main>
<div class="dots" id="dots"></div>

<section class="step on">
<div class="big">💌</div>
<h1>Will you go out with me?</h1>
<p>I put together a little something. Just one question.</p>
<div class="row">
<button class="btn" id="yes">Yes! 💖</button>
<button class="btn ghost" id="no">No</button>
</div>
</section>

<section class="step">
<div class="big">📅</div>
<h1>Pick our day</h1>
<p>When are you free?</p>
<input type="date" id="date">
<input type="time" id="time" value="19:00">
<button class="btn" data-next disabled id="n1">Next</button>
</section>

<section class="step">
<div class="big">🍽️</div>
<h1>What are we eating?</h1>
<p>Choose one.</p>
<div class="grid" data-key="food">
<button class="opt" aria-pressed="false"><span>🍕</span>Pizza</button>
<button class="opt" aria-pressed="false"><span>🍣</span>Sushi</button>
<button class="opt" aria-pressed="false"><span>🍔</span>Burgers</button>
<button class="opt" aria-pressed="false"><span>🍦</span>Dessert only</button>
</div>
<button class="btn" data-next disabled>Next</button>
</section>

<section class="step">
<div class="big">🎟️</div>
<h1>Then what?</h1>
<p>Choose our activity.</p>
<div class="grid" data-key="act">
<button class="opt" aria-pressed="false"><span>🎬</span>Movie</button>
<button class="opt" aria-pressed="false"><span>🌅</span>Sunset walk</button>
<button class="opt" aria-pressed="false"><span>🎳</span>Bowling</button>
<button class="opt" aria-pressed="false"><span>🎮</span>Arcade</button>
</div>
<button class="btn" data-next disabled>See our plan</button>
</section>

<section class="step">
<div class="big">🎉</div>
<h1>It's a date!</h1>
<p>Here's our plan.</p>
<div class="sum" id="sum"></div>
<button class="btn" id="copy">Copy plan to send back</button>
<p id="msg" style="margin-top:14px;min-height:1.5em"></p>
</section>
</main>

<script>
var YOUR_NAME = "Me";
var d = {}, cur = 0, total = 5;
var steps = document.querySelectorAll('.step');
var dots = document.getElementById('dots');
for (var i = 0; i < total; i++) dots.appendChild(document.createElement('i'));
function go(n){
  cur = n;
  steps.forEach(function(s,i){ s.classList.toggle('on', i === n); });
  dots.querySelectorAll('i').forEach(function(x,i){ x.classList.toggle('f', i <= n); });
  if (n === 4) summary();
}
go(0);

var no = document.getElementById('no'), yes = document.getElementById('yes');
var tries = 0, lines = ["No", "Are you sure?", "Really?", "Try again 🥺", "Nope, not allowed", "Just say yes 😌"];
function jump(){
  tries++;
  no.textContent = lines[Math.min(tries, lines.length - 1)];
  no.classList.add('run');
  var w = no.offsetWidth, h = no.offsetHeight, m = 12;
  var vw = window.innerWidth, vh = window.innerHeight;
  var ox = no.offsetLeft, oy = no.offsetTop, x, y, t = 0;
  do {
    x = m + Math.random() * (vw - w - 2*m);
    y = m + Math.random() * (vh - h - 2*m);
    t++;
  } while (t < 30 && Math.hypot(x - ox, y - oy) < 160);
  no.style.left = x + "px";
  no.style.top = y + "px";
  yes.style.transform = "scale(" + Math.min(1 + tries * .1, 1.6) + ")";
}
['pointerenter','pointerdown','mousedown','touchstart','focus'].forEach(function(ev){
  no.addEventListener(ev, function(e){ e.preventDefault(); jump(); }, {passive:false});
});
no.addEventListener('click', function(e){ e.preventDefault(); e.stopPropagation(); jump(); });
document.addEventListener('pointermove', function(e){
  if (!no.classList.contains('run')) return;
  var r = no.getBoundingClientRect();
  if (Math.hypot(e.clientX - (r.left + r.width/2), e.clientY - (r.top + r.height/2)) < 90) jump();
});
yes.addEventListener('click', function(){ go(1); });

var dateEl = document.getElementById('date'), timeEl = document.getElementById('time');
var n1 = document.getElementById('n1');
try { dateEl.min = new Date().toISOString().slice(0,10); } catch(e){}
function chk(){ n1.disabled = !dateEl.value || !timeEl.value; }
dateEl.addEventListener('input', chk); timeEl.addEventListener('input', chk);

document.querySelectorAll('.grid').forEach(function(g){
  var next = g.parentNode.querySelector('[data-next]');
  g.querySelectorAll('.opt').forEach(function(b){
    b.addEventListener('click', function(){
      g.querySelectorAll('.opt').forEach(function(o){ o.setAttribute('aria-pressed','false'); });
      b.setAttribute('aria-pressed','true');
      d[g.dataset.key] = b.textContent.trim();
      next.disabled = false;
    });
  });
});
document.querySelectorAll('[data-next]').forEach(function(b){
  b.addEventListener('click', function(){ go(cur + 1); });
});

function fmtDate(){
  var p = dateEl.value.split('-');
  var dt = new Date(+p[0], p[1]-1, +p[2]);
  return dt.toLocaleDateString(undefined, {weekday:'long', month:'long', day:'numeric'});
}
function fmtTime(){
  var t = timeEl.value.split(':'), h = +t[0];
  return ((h % 12) || 12) + ':' + t[1] + ' ' + (h < 12 ? 'AM' : 'PM');
}
function plan(){
  return "It's a date! 💖\nDay: " + fmtDate() + "\nTime: " + fmtTime() + "\nFood: " + d.food + "\nThen: " + d.act;
}
function summary(){
  var rows = [["Day", fmtDate()], ["Time", fmtTime()], ["Food", d.food], ["Then", d.act]];
  document.getElementById('sum').innerHTML = rows.map(function(r){
    return "<div><b>" + r[0] + "</b><span>" + r[1] + "</span></div>";
  }).join('');
}
document.getElementById('copy').addEventListener('click', function(){
  var m = document.getElementById('msg');
  try {
    navigator.clipboard.writeText(plan()).then(function(){ m.textContent = "Copied. Send it to " + YOUR_NAME + "!"; },
      function(){ m.textContent = "Couldn't copy. Screenshot this page instead."; });
  } catch(e){ m.textContent = "Couldn't copy. Screenshot this page instead."; }
});
</script>
</body>
</html>
