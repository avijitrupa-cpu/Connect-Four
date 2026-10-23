# Connect-Four
Connect Four Game
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<meta name="theme-color" content="#111827">
<title>Connect Four — 2 Minute Demo</title>
<style>
  :root{--bg:#0b1020;--panel:#151d32;--text:#f8fafc;--muted:#aab5cc;--yellow:#facc15;--red:#fb5252;--blue:#5b8cff;--green:#34d399}
  *{box-sizing:border-box}
  body{margin:0;min-height:100vh;background:radial-gradient(circle at top,#1d2b50 0,#0b1020 55%);font-family:system-ui,-apple-system,Segoe UI,Roboto,sans-serif;color:var(--text);display:flex;justify-content:center;padding:22px}
  .app{width:min(100%,760px)}
  header{text-align:center;margin:6px 0 20px}
  h1{margin:0;font-size:clamp(2rem,6vw,3rem);letter-spacing:-1px}
  header p{color:var(--muted);margin:8px 0}
  .topbar{display:flex;align-items:center;justify-content:space-between;gap:12px;flex-wrap:wrap;background:var(--panel);border:1px solid #293653;border-radius:16px;padding:12px 16px;margin-bottom:14px}
  .status{font-weight:700}.timer{font-variant-numeric:tabular-nums;font-size:1.25rem;font-weight:800;color:var(--yellow)}
  .board-wrap{background:linear-gradient(145deg,#2563eb,#1d4ed8);padding:clamp(8px,2.5vw,16px);border-radius:20px;box-shadow:0 18px 45px #0007;position:relative}
  .board{display:grid;grid-template-columns:repeat(7,minmax(0,1fr));gap:clamp(5px,1.5vw,11px)}
  .cell{aspect-ratio:1;border:0;border-radius:50%;background:#0b1020;box-shadow:inset 0 5px 10px #0009;display:flex;align-items:center;justify-content:center;padding:0;cursor:pointer;transition:transform .12s}
  .cell:hover{transform:scale(.96);outline:2px solid #ffffff80}
  .disc{width:100%;height:100%;border-radius:50%;background:#0b1020;display:block}
  .disc.red{background:radial-gradient(circle at 32% 27%,#ff9b9b,var(--red) 52%,#b91c1c)}
  .disc.yellow{background:radial-gradient(circle at 32% 27%,#fff6a0,var(--yellow) 52%,#ca8a04)}
  .controls{display:flex;gap:10px;justify-content:center;flex-wrap:wrap;margin:16px 0}
  button,.paylink{font:inherit;font-weight:750;border:0;border-radius:12px;padding:11px 16px;cursor:pointer;text-decoration:none;display:inline-flex;align-items:center;justify-content:center}
  .primary{background:var(--yellow);color:#171717}.secondary{background:#26334f;color:var(--text);border:1px solid #3a4b70}
  .hint{text-align:center;color:var(--muted);font-size:.93rem}
  .panel{margin-top:18px;background:var(--panel);border:1px solid #293653;border-radius:18px;padding:18px}
  .panel h2{margin:0 0 8px}.price{font-size:2rem;font-weight:900;color:var(--yellow);margin:5px 0}
  .upi{font-family:ui-monospace,SFMono-Regular,monospace;word-break:break-all;background:#0b1020;padding:10px;border-radius:10px;display:inline-block}
  .pay-actions{display:flex;gap:10px;flex-wrap:wrap;margin:12px 0}
  #qr{background:#fff;padding:10px;width:220px;height:220px;border-radius:12px;display:flex;align-items:center;justify-content:center;margin:12px auto}
  #qr img,#qr canvas{max-width:100%;height:auto!important}
  .overlay{position:fixed;inset:0;background:#050914eF;z-index:5;display:none;align-items:center;justify-content:center;padding:18px}
  .overlay.show{display:flex}.modal{width:min(100%,440px);background:var(--panel);border:1px solid #3a4b70;border-radius:20px;padding:24px;text-align:center;box-shadow:0 20px 80px #000b}
  .modal h2{margin-top:0}.small{font-size:.85rem;color:var(--muted);line-height:1.5}
  .notice{font-size:.85rem;color:#cbd5e1;line-height:1.5;border-left:3px solid var(--yellow);padding-left:10px;margin-top:14px}
  .hidden{display:none!important}
  @media(max-width:430px){body{padding:12px}.topbar{padding:10px}.panel{padding:14px}button,.paylink{padding:10px 12px}}
</style>
</head>
<body>
<main class="app">
  <header>
    <h1>🔴 Connect Four 🟡</h1>
    <p>Drop four discs in a row to win. Play against a friend on one device.</p>
  </header>
  <div class="topbar">
    <div class="status" id="status" aria-live="polite">Red's turn</div>
    <div>Demo time: <span class="timer" id="timer">02:00</span></div>
  </div>
  <section class="board-wrap" aria-label="Connect Four board">
    <div class="board" id="board"></div>
  </section>
  <div class="controls">
    <button class="primary" id="newGame">New game</button>
    <button class="secondary" id="soundToggle" aria-pressed="false">Sound: Off</button>
  </div>
  <p class="hint">Tap any column to drop a disc. First player to connect 4 horizontally, vertically, or diagonally wins.</p>

  <section class="panel" id="payment">
    <h2>Unlock the full game</h2>
    <div class="price">₹100 <span style="font-size:1rem;color:var(--muted);font-weight:600">one-time payment</span></div>
    <p>Pay using any UPI app to the UPI ID below.</p>
    <p><strong>UPI ID:</strong> <span class="upi" id="upiText">adey6547@axl</span></p>
    <div class="pay-actions">
      <a class="paylink primary" id="payLink" href="upi://pay?pa=adey6547%40axl&amp;pn=Connect%20Four%20Game&amp;am=100.00&amp;cu=INR&amp;tn=Connect%20Four%20Full%20Game" target="_blank" rel="noopener">Pay ₹100 with UPI</a>
      <button class="secondary" id="copyUpi">Copy UPI ID</button>
    </div>
    <div id="qr" aria-label="UPI payment QR code"><span style="color:#333">Loading QR code…</span></div>
    <p class="small">Scan the QR code with a UPI app. Confirm the recipient and ₹100 amount in your payment app before paying.</p>
    <p class="notice">Demo only: this single-file page cannot verify a payment or securely grant paid access. The payment button and QR open a payment request; they do not confirm that money was received. For real paid access, connect a trusted payment gateway and verify payment on a server.</p>
  </section>
</main>

<div class="overlay" id="lockOverlay" role="dialog" aria-modal="true" aria-labelledby="lockTitle">
  <div class="modal">
    <h2 id="lockTitle">⏱️ Demo time is over</h2>
    <p>Your 2-minute demo has ended. Pay ₹100 to support the full game experience.</p>
    <p><strong>UPI ID:</strong><br><span class="upi">adey6547@axl</span></p>
    <a class="paylink primary" href="upi://pay?pa=adey6547%40axl&amp;pn=Connect%20Four%20Game&amp;am=100.00&amp;cu=INR&amp;tn=Connect%20Four%20Full%20Game">Pay ₹100 with UPI</a>
    <p class="small">Use the QR code on the page to scan and pay. This demo does not automatically verify payments.</p>
    <button class="secondary" id="closeOverlay">Close</button>
  </div>
</div>
<script>
(() => {
  const ROWS=6, COLS=7, DEMO_SECONDS=120;
  const boardEl=document.getElementById('board'), statusEl=document.getElementById('status');
  const timerEl=document.getElementById('timer'), overlay=document.getElementById('lockOverlay');
  let grid=[], current='red', active=true, seconds=DEMO_SECONDS, timerId=null, soundOn=false;
  const labels={red:'Red',yellow:'Yellow'};
  function initBoard(){
    grid=Array.from({length:ROWS},()=>Array(COLS).fill(''));
    boardEl.innerHTML='';
    for(let r=0;r<ROWS;r++) for(let c=0;c<COLS;c++){
      const b=document.createElement('button'); b.className='cell'; b.type='button';
      b.setAttribute('aria-label',`Row ${r+1}, column ${c+1}, empty`);
      b.dataset.col=c; b.innerHTML='<span class="disc"></span>';
      b.addEventListener('click',()=>drop(c)); boardEl.appendChild(b);
    }
    current='red'; active=true; statusEl.textContent="Red's turn";
    render();
  }
  function render(){
    [...boardEl.children].forEach((cell,i)=>{
      const r=Math.floor(i/COLS),c=i%COLS,disc=cell.firstElementChild;
      disc.className='disc'+(grid[r][c]?' '+grid[r][c]:'');
      cell.setAttribute('aria-label',`Row ${r+1}, column ${c+1}, ${grid[r][c]||'empty'}`);
      cell.disabled=!active || seconds<=0 || grid[0][c]!=='';
    });
  }
  function drop(c){
    if(!active||seconds<=0||grid[0][c])return;
    let r=ROWS-1; while(r>=0&&grid[r][c])r--;
    if(r<0)return; grid[r][c]=current;
    if(soundOn)beep();
    if(wins(r,c,current)){active=false;statusEl.textContent=`${labels[current]} wins! 🎉`;render();return;}
    if(grid.every(row=>row.every(Boolean))){active=false;statusEl.textContent="It's a draw!";render();return;}
    current=current==='red'?'yellow':'red';statusEl.textContent=`${labels[current]}'s turn`;render();
  }
  function wins(r,c,p){
    const dirs=[[0,1],[1,0],[1,1],[1,-1]];
    return dirs.some(([dr,dc])=>{
      let count=1;
      for(const sign of [-1,1]){let rr=r+dr*sign,cc=c+dc*sign;while(rr>=0&&rr<ROWS&&cc>=0&&cc<COLS&&grid[rr][cc]===p){count++;rr+=dr*sign;cc+=dc*sign;}}
      return count>=4;
    });
  }
  function formatTime(s){return `${String(Math.floor(s/60)).padStart(2,'0')}:${String(s%60).padStart(2,'0')}`;}
  function updateTimer(){timerEl.textContent=formatTime(seconds);if(seconds<=0){active=false;statusEl.textContent='Demo ended — unlock full game';render();overlay.classList.add('show');clearInterval(timerId);return;}seconds--;timerEl.textContent=formatTime(seconds);}
  function startTimer(){clearInterval(timerId);timerId=setInterval(updateTimer,1000);}
  function reset(){seconds=DEMO_SECONDS;timerEl.textContent='02:00';overlay.classList.remove('show');initBoard();startTimer();}
  function beep(){try{const ctx=new(window.AudioContext||window.webkitAudioContext)();const o=ctx.createOscillator(),g=ctx.createGain();o.connect(g);g.connect(ctx.destination);o.frequency.value=440;g.gain.value=.06;o.start();o.stop(ctx.currentTime+.07);o.onended=()=>ctx.close();}catch(e){}}
  document.getElementById('newGame').addEventListener('click',reset);
  document.getElementById('closeOverlay').addEventListener('click',()=>overlay.classList.remove('show'));
  document.getElementById('soundToggle').addEventListener('click',e=>{soundOn=!soundOn;e.currentTarget.textContent=`Sound: ${soundOn?'On':'Off'}`;e.currentTarget.setAttribute('aria-pressed',String(soundOn));});
  document.getElementById('copyUpi').addEventListener('click',async()=>{
    try{await navigator.clipboard.writeText('adey6547@axl');document.getElementById('copyUpi').textContent='Copied!';}
    catch(e){const t=document.createElement('textarea');t.value='adey6547@axl';document.body.appendChild(t);t.select();document.execCommand('copy');t.remove();document.getElementById('copyUpi').textContent='Copied!';}
    setTimeout(()=>document.getElementById('copyUpi').textContent='Copy UPI ID',1600);
  });
  // QRCode.js renders a QR containing a standard UPI payment URI.
  const qrText='upi://pay?pa=adey6547%40axl&pn=Connect%20Four%20Game&am=100.00&cu=INR&tn=Connect%20Four%20Full%20Game';
  const qrScript=document.createElement('script');
  qrScript.src='https://cdnjs.cloudflare.com/ajax/libs/qrcodejs/1.0.0/qrcode.min.js';
  qrScript.onload=()=>{const q=document.getElementById('qr');q.innerHTML='';new QRCode(q,{text:qrText,width:200,height:200,colorDark:'#111111',colorLight:'#ffffff',correctLevel:QRCode.CorrectLevel.H});};
  qrScript.onerror=()=>{document.getElementById('qr').innerHTML='<span style="color:#333;text-align:center;font-size:13px">QR library unavailable.<br>Use “Pay ₹100 with UPI”.</span>';};
  document.head.appendChild(qrScript);
  initBoard();startTimer();
})();
</script>
</body>
</html>

