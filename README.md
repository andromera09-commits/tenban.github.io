<!doctype html>
<html lang="vi">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>Web Troll - Nhấn vào đây</title>
  <style>
    :root{--bg:#0f172a;--card:#0b1220;--accent:#ff4d6d}
    *{box-sizing:border-box}
    html,body{height:100%;margin:0;font-family:Inter, system-ui, -apple-system, 'Segoe UI', Roboto, 'Helvetica Neue', Arial}
    body{display:flex;align-items:center;justify-content:center;background:linear-gradient(135deg,#071428 0%, #0b1b2b 100%);color:#e6eef8}
    .wrapper{width:92%;max-width:720px;padding:28px;border-radius:14px;background:linear-gradient(180deg, rgba(255,255,255,0.02), transparent);box-shadow:0 8px 30px rgba(2,6,23,0.6);text-align:center}
    h1{margin:0 0 12px;font-size:22px}
    p{margin:0 0 18px;color:#a8bed9}
    .btn{appearance:none;border:0;background:linear-gradient(90deg,var(--accent),#ff8a66);padding:12px 20px;border-radius:12px;font-weight:700;color:white;cursor:pointer;box-shadow:0 6px 18px rgba(255,77,109,0.18);transition:transform .12s ease}
    .btn:active{transform:translateY(2px)}
    .message{position:fixed;left:50%;top:50%;transform:translate(-50%,-50%) scale(0.6);background:radial-gradient(circle at 10% 10%, rgba(255,255,255,0.06), transparent), rgba(255,255,255,0.02);padding:28px 32px;border-radius:14px;color:var(--accent);font-size:28px;font-weight:800;backdrop-filter:blur(6px);display:flex;align-items:center;gap:14px;border:2px solid rgba(255,77,109,0.12);opacity:0;pointer-events:none}
    .message.show{animation:pop .9s cubic-bezier(.2,.9,.2,1) forwards}
    @keyframes pop{0%{opacity:0;transform:translate(-50%,-50%) scale(.6)}60%{opacity:1;transform:translate(-50%,-50%) scale(1.08)}100%{opacity:1;transform:translate(-50%,-50%) scale(1)}}
    .close{background:none;border:0;color:#93b8d9;font-weight:700;cursor:pointer;padding:6px 8px;border-radius:8px}
    .hint{margin-top:14px;color:#96adc6;font-size:13px}
    /* small confetti dots */
    .confetti{position:fixed;left:0;top:0;right:0;bottom:0;pointer-events:none;overflow:hidden}
    .confetti i{position:absolute;width:8px;height:8px;border-radius:2px;opacity:0;transform:translateY(-10px);animation:fall 1500ms linear forwards}
    @keyframes fall{to{opacity:1;transform:translateY(110vh) rotate(600deg)}}
    footer{margin-top:16px;color:#90a7c3;font-size:13px}
  </style>
</head>
<body>
  <div class="wrapper">
    <h1>Chơi khăm nhẹ cho bạn bè</h1>
    <p>Nhấn nút bên dưới — sẽ xuất hiện một thông điệp bất ngờ cho ai vừa bấm.</p>
    <button id="trollBtn" class="btn">Nhấn vào đây</button>
    <div class="hint">(Hoặc bấm bất kỳ chỗ nào trên trang)</div>
    <footer>Tip: lưu file này thành <code>troll-friends.html</code> rồi mở bằng trình duyệt.</footer>
  </div>

  <div id="message" class="message" aria-hidden="true">
    <span id="text">các chú chó vừa bấm vào đây</span>
    <button id="close" class="close" title="Đóng">✕</button>
  </div>

  <div id="confetti" class="confetti" aria-hidden="true"></div>

  <script>
    const btn = document.getElementById('trollBtn');
    const message = document.getElementById('message');
    const closeBtn = document.getElementById('close');
    const confetti = document.getElementById('confetti');
    const textEl = document.getElementById('text');

    // exact text requested
    const PHRASE = 'các chú chó vừa bấm vào đây';

    function showMessage(x, y){
      textEl.textContent = PHRASE;
      // optionally position message near click (keeps centered by default)
      message.style.left = '50%';
      message.style.top = '50%';
      message.classList.remove('show');
      // small delay to restart animation
      void message.offsetWidth;
      message.classList.add('show');
      message.setAttribute('aria-hidden', 'false');
      spawnConfetti();
    }

    function hideMessage(){
      message.classList.remove('show');
      message.setAttribute('aria-hidden','true');
    }

    btn.addEventListener('click', (e)=>{ e.preventDefault(); showMessage(); });
    closeBtn.addEventListener('click', hideMessage);
    // click anywhere to trigger (but ignore clicks on the close button)
    document.addEventListener('click', (e)=>{
      if(e.target === closeBtn || e.target === btn) return;
      showMessage();
    });

    // confetti generator (lightweight)
    function spawnConfetti(){
      confetti.innerHTML = '';
      const colors = ['#ff4d6d','#ffd166','#6ce4ff','#a0ff7a','#b39cff'];
      for(let i=0;i<30;i++){
        const el = document.createElement('i');
        el.style.left = Math.random()*100 + '%';
        el.style.top = Math.random()*-20 + 'vh';
        el.style.width = (6 + Math.random()*8) + 'px';
        el.style.height = (6 + Math.random()*8) + 'px';
        el.style.background = colors[Math.floor(Math.random()*colors.length)];
        el.style.transform = 'translateY(-10px) rotate('+Math.floor(Math.random()*360)+'deg)';
        el.style.animationDuration = (1000 + Math.random()*1200) + 'ms';
        confetti.appendChild(el);
      }
      // remove after animation
      setTimeout(()=> confetti.innerHTML = '', 2200);
    }

    // keyboard shortcut: press T to trigger
    document.addEventListener('keydown', (e)=>{ if(e.key.toLowerCase()==='t') showMessage(); });
  </script>
</body>
</html>

