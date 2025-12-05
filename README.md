‎<!DOCTYPE html>
‎<html lang="hi">
‎<head>
‎<meta charset="utf-8" />
‎<meta name="viewport" content="width=device-width,initial-scale=1" />
‎<title>I Love You — क्लिक करो</title>
‎<style>
‎  :root{
‎    --bg:#0f1724;
‎    --card:#0b1020;
‎    --accent:#ff3b6b;
‎    --glass: rgba(255,255,255,0.03);
‎  }
‎  html,body{height:100%;margin:0;font-family:system-ui,-apple-system,Segoe UI,Roboto,"Noto Sans",Arial;}
‎  body{
‎    background: radial-gradient(1200px 700px at 10% 10%, #102028 0%, var(--bg) 30%, #051023 100%);
‎    display:flex;align-items:center;justify-content:center;padding:24px;color:#e6eef8;
‎  }
‎
‎  .card{
‎    width:100%;max-width:480px;padding:28px;border-radius:18px;background:linear-gradient(180deg, rgba(255,255,255,0.02), rgba(255,255,255,0.01));
‎    box-shadow: 0 8px 30px rgba(2,6,23,0.6); text-align:center;
‎    backdrop-filter: blur(6px);
‎  }
‎  h1{margin:0 0 12px 0;font-size:20px;letter-spacing:0.3px;color:#cfe7ff;}
‎  .note{font-size:13px;color:#98b6d6;margin-bottom:18px}
‎
‎  .btn{
‎    appearance:none;border:0;background:linear-gradient(90deg,#ff6b8b,#ff3b6b);color:white;
‎    padding:12px 20px;border-radius:12px;font-weight:700;font-size:16px;cursor:pointer;
‎    box-shadow: 0 6px 18px rgba(255,59,107,0.18); transition:transform .12s ease, box-shadow .12s;
‎  }
‎  .btn:active{transform:translateY(2px)}
‎  .message{
‎    margin-top:26px;font-size:44px;line-height:1;color:var(--accent);
‎    font-weight:800;opacity:0;transform:scale(.9) translateY(6px);
‎    transition: opacity .45s cubic-bezier(.2,.9,.3,1), transform .45s cubic-bezier(.2,.9,.3,1);
‎    user-select:none;
‎    text-shadow: 0 6px 18px rgba(255,55,100,0.12), 0 2px 6px rgba(0,0,0,0.45);
‎  }
‎
‎  .message.show{opacity:1;transform:scale(1) translateY(0)}
‎
‎  /* small floating hearts container (absolute) */
‎  .hearts{
‎    position:fixed;left:0;top:0;right:0;bottom:0;pointer-events:none;overflow:hidden;z-index:1000;
‎  }
‎  .heart{
‎    position:absolute; font-size:22px; opacity:0; transform:translateY(0) scale(1);
‎    will-change:transform,opacity;
‎    filter:drop-shadow(0 6px 10px rgba(0,0,0,0.35));
‎  }
‎
‎  /* Subtle footer / credit */
‎  .small{margin-top:18px;font-size:12px;color:#93a9c9}
‎</style>
‎</head>
‎<body>
‎  <div class="card" role="main" aria-live="polite">
‎    <h1>एक छोटा सा सन्देश</h1>
‎    <div class="note">नीचे के बटन पर क्लिक करो — प्यार दिखेगा 💌</div>
‎
‎    <button class="btn" id="showBtn" aria-controls="msg" aria-pressed="false">क्लिक करें</button>
‎
‎    <div id="msg" class="message" role="status" aria-hidden="true">I ❤️ YOU</div>
‎
‎    <div class="small">यह पेज ऑफ़लाइन चलता है — फ़ाइल सेव करके खोलो।</div>
‎  </div>
‎
‎  <div class="hearts" id="hearts"></div>
‎
‎<script>
‎(function(){
‎  const btn = document.getElementById('showBtn');
‎  const msg = document.getElementById('msg');
‎  const heartsContainer = document.getElementById('hearts');
‎  let shown = false;
‎
‎  function createHeart(xPercent){
‎    const h = document.createElement('div');
‎    h.className = 'heart';
‎    // choose a heart emoji variant
‎    const hearts = ['❤','💖','💘','💓','💕','💗'];
‎    h.textContent = hearts[Math.floor(Math.random()*hearts.length)];
‎    // randomize size
‎    const size = 16 + Math.floor(Math.random()*22);
‎    h.style.fontSize = size + 'px';
‎    // start position based on click x% or random
‎    const left = (typeof xPercent === 'number') ? xPercent : Math.random()*100;
‎    h.style.left = left + '%';
‎    // initial bottom offset
‎    const bottomStart = -30 - Math.random()*30;
‎    h.style.bottom = bottomStart + 'px';
‎
‎    // animation values
‎    const rise = 80 + Math.random()*40; // px relative
‎    const drift = (Math.random()*80) - 40; // px drift left/right
‎    const duration = 1400 + Math.random()*1200; // ms
‎    heartsContainer.appendChild(h);
‎
‎    // animate with requestAnimationFrame using CSS transform + opacity
‎    // we will create a simple inline animation via CSS transitions
‎    // set initial styles for transition
‎    h.style.transition = `transform ${duration}ms cubic-bezier(.2,.9,.2,1), opacity ${duration}ms linear`;
‎    // cause layout and then set final transform
‎    requestAnimationFrame(()=> {
‎      const translateY = -(rise + (Math.random()*30));
‎      const translateX = drift;
‎      const rotate = (Math.random()*60)-30;
‎      h.style.transform = `translate(${translateX}px, ${translateY}px) rotate(${rotate}deg) scale(1.05)`;
‎      h.style.opacity = '1';
‎    });
‎
‎    // fade out near end
‎    setTimeout(()=> { h.style.opacity = '0'; }, duration - 250);
‎
‎    // remove after animation
‎    setTimeout(()=> { if(h && h.parentNode) h.parentNode.removeChild(h); }, duration + 50);
‎  }
‎
‎  function burst(xPercent){
‎    // create several hearts quickly
‎    const count = 8 + Math.floor(Math.random()*8);
‎    for(let i=0;i<count;i++){
‎      // stagger creation slightly
‎      setTimeout(()=> createHeart(xPercent), i*40);
‎    }
‎  }
‎
‎  btn.addEventListen
