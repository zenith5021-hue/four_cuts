<!DOCTYPE html>
<html lang="ko">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>공감 네컷 | 양구초등학교</title>
<meta name="description" content="양구초등학교 공감 네컷 - 손동작으로 공감을 표현하는 포토부스">
<style>
  :root{
    --coral:#ff6b81;
    --coral-dark:#e0526a;
    --cream:#fff6ef;
    --navy:#2d2a3d;
    --yellow:#ffd166;
  }
  *{box-sizing:border-box;}
  body{
    margin:0;
    font-family:'Apple SD Gothic Neo','Malgun Gothic',sans-serif;
    background:var(--cream);
    background-image: radial-gradient(circle at 10% 10%, #ffe1e8 0%, transparent 40%),
                       radial-gradient(circle at 90% 90%, #ffe9c7 0%, transparent 40%);
    min-height:100vh;
    display:flex;
    align-items:center;
    justify-content:center;
    padding:24px;
    color:var(--navy);
  }
  .app{ width:100%; max-width:520px; }
  .screen{ display:none; animation: fadeIn .25s ease; }
  .screen.active{display:block;}
  @keyframes fadeIn{from{opacity:0; transform:translateY(6px);} to{opacity:1; transform:translateY(0);}}

  .brandbar{
    display:flex;
    align-items:center;
    justify-content:center;
    gap:10px;
    margin-bottom:6px;
  }
  .brandbar img{ height:40px; }
  .brandbar span{ font-size:14px; color:#8a8399; font-weight:600; }

  .logo{ text-align:center; margin-bottom:8px; }
  .logo h1{
    font-size:38px; margin:0; color:var(--coral);
    letter-spacing:-1px;
    text-shadow: 2px 2px 0 #fff, 4px 4px 0 rgba(0,0,0,0.05);
  }
  .logo p{ margin:6px 0 0; font-size:15px; color:#7a7488; }

  .card{
    background:#fff; border-radius:24px; padding:28px 24px;
    box-shadow:0 10px 30px rgba(224,82,106,0.12); margin-top:16px;
  }
  .btn{
    display:block; width:100%; padding:16px; border:none; border-radius:16px;
    font-size:17px; font-weight:700; cursor:pointer;
    transition:transform .15s ease, box-shadow .15s ease;
  }
  .btn:active{transform:scale(0.97);}
  .btn:disabled{opacity:.5; cursor:not-allowed;}
  .btn-primary{ background:var(--coral); color:#fff; box-shadow:0 6px 16px rgba(255,107,129,0.4); }
  .btn-ghost{ background:var(--yellow); color:var(--navy); margin-top:12px; box-shadow:0 6px 16px rgba(255,209,102,0.4); }
  .btn-secondary{ background:#f1eef7; color:var(--navy); }
  .hint{ text-align:center; font-size:13px; color:#9a94a8; margin-top:16px; line-height:1.6; }

  .topbar{ display:flex; align-items:center; justify-content:space-between; margin-bottom:14px; }
  .back{ background:#fff; border:none; border-radius:20px; padding:8px 16px; font-weight:600; cursor:pointer; box-shadow:0 3px 10px rgba(0,0,0,0.08); }
  .counter{ background:var(--coral); color:#fff; padding:6px 14px; border-radius:20px; font-weight:700; font-size:14px; }

  .cam-wrap{ position:relative; border-radius:20px; overflow:hidden; background:#111; aspect-ratio:4/3; box-shadow:0 10px 30px rgba(0,0,0,0.25); }
  video, canvas.overlay-canvas{ width:100%; height:100%; object-fit:cover; display:block; }
  video{ transform:scaleX(-1); }
  canvas.overlay-canvas{ position:absolute; inset:0; transform:scaleX(-1); pointer-events:none; }

  .overlay-emoji{
    position:absolute; top:6%; left:50%; transform:translateX(-50%);
    font-size:60px; pointer-events:none; opacity:0; transition:opacity .2s ease;
    text-shadow:0 4px 12px rgba(0,0,0,.35);
  }
  .overlay-emoji.show{ opacity:1; animation: pop .5s ease; }
  @keyframes pop{
    0%{transform:translateX(-50%) scale(0.4) rotate(-8deg);}
    60%{transform:translateX(-50%) scale(1.15) rotate(4deg);}
    100%{transform:translateX(-50%) scale(1) rotate(0);}
  }
  .gesture-tag{
    position:absolute; bottom:10px; left:10px;
    background:rgba(0,0,0,.55); color:#fff; font-size:13px; font-weight:600;
    padding:6px 12px; border-radius:20px; pointer-events:none;
  }
  .countdown{
    position:absolute; inset:0; display:flex; align-items:center; justify-content:center;
    font-size:96px; font-weight:800; color:#fff; background:rgba(0,0,0,0.25);
    opacity:0; pointer-events:none; text-shadow:0 4px 10px rgba(0,0,0,0.4);
  }
  .countdown.show{opacity:1;}
  .flash{ position:absolute; inset:0; background:#fff; opacity:0; pointer-events:none; }
  .flash.on{ opacity:.85; transition:opacity .08s ease; }

  .shots-strip{ display:flex; gap:8px; margin-top:10px; }
  .shots-strip .slot{ flex:1; aspect-ratio:1; background:#f1eef7; border-radius:10px; overflow:hidden; border:2px solid #fff; }
  .shots-strip .slot img{width:100%;height:100%;object-fit:cover;}

  .gesture-legend{ display:grid; grid-template-columns:repeat(4,1fr); gap:8px; margin-top:14px; }
  .gesture-legend .g{ background:#fff; border:2px solid #f1eef7; border-radius:14px; padding:8px 4px; font-size:11px; font-weight:600; text-align:center; line-height:1.4; }
  .gesture-legend .g span{ display:block; font-size:20px; margin-bottom:2px; }
  .gesture-legend .g.active{ border-color:var(--coral); background:#fff0f3; color:var(--coral-dark); }

  .target-banner{
    background:var(--coral); color:#fff; text-align:center; font-weight:700;
    font-size:15px; padding:10px; border-radius:14px; margin-top:10px;
  }
  .hold-bar{ height:8px; background:rgba(255,255,255,.35); border-radius:8px; margin-top:8px; overflow:hidden; }
  .hold-bar-fill{ height:100%; width:0%; background:#fff; transition:width .08s linear; }

  .shoot-row{ display:flex; gap:10px; margin-top:14px; }
  .shoot-row .btn{flex:1;}

  #resultCanvasWrap{ display:flex; justify-content:center; margin-bottom:16px; }
  #resultCanvasWrap canvas{ max-width:100%; border-radius:16px; box-shadow:0 10px 30px rgba(0,0,0,0.18); }
  .result-actions{ display:flex; gap:10px; }
  .result-actions .btn{flex:1;}

  .loading-msg{ text-align:center; color:#9a94a8; font-size:13px; margin-top:8px; }
</style>
</head>
<body>
<div class="app">

  <!-- HOME -->
  <div class="screen active" id="screen-home">
    <div class="brandbar">
      <img src="assets/logo.png" alt="양구초등학교">
    </div>
    <div class="logo">
      <h1>🌸 공감 네컷</h1>
      <p>우리 반, 서로를 응원하는 마음을 담아 찍어요</p>
    </div>
    <div class="card">
      <button class="btn btn-primary" onclick="goTo('screen-camera')">📸 사진 찍으러 가기</button>
      <button class="btn btn-ghost" onclick="goTo('screen-help')">❓ 사용 방법</button>
    </div>
    <p class="hint">가로 2×2 그리드 프레임 · 총 4컷 연속 촬영<br>카메라 권한을 허용해주세요 · 손동작을 자동으로 인식해요</p>
  </div>

  <!-- HELP -->
  <div class="screen" id="screen-help">
    <div class="card">
      <h2 style="color:var(--coral); margin-top:0;">🫶 공감 네컷 사용방법</h2>
      <p><b>1. 카메라 허용</b><br>브라우저 상단에서 카메라 권한을 허용해주세요.</p>
      <p><b>2. 손동작으로 표현</b><br>화면에 안내되는 순서대로 동작을 보여주면 자동으로 촬영돼요.</p>
      <ul style="padding-left:18px; line-height:1.9;">
        <li>👍 엄지척 — 응원해요</li>
        <li>✌️ 브이 — 우리반 최고</li>
        <li>🤟 아이러브유(엄지·검지·새끼) — 사랑해요</li>
        <li>✋ 손바닥 펴기 — 함께해요</li>
      </ul>
      <p><b>3. 촬영</b><br>동작이 인식되면 5초 카운트다운 후 촬영, 총 4번 반복됩니다.</p>
      <div style="background:#fff7ee; border:1px solid #ffe1c2; border-radius:14px; padding:14px 16px; margin-top:14px;">
        <p style="margin:0 0 6px; font-weight:700; color:#e0526a;">📌 인식이 잘 되려면</p>
        <ul style="padding-left:18px; margin:0; line-height:1.8; font-size:14px;">
          <li>밝은 곳에서 촬영하세요 (역광 X)</li>
          <li>손과 얼굴이 화면에 크게, 뚜렷하게 나오도록 카메라에서 30~80cm 거리를 유지하세요</li>
          <li>동작을 취한 손 하나만 화면 중앙 쪽에 크게 보여주세요 (다른 손이나 다른 사람 손은 화면 밖으로)</li>
          <li>동작을 짧게 흔들지 말고 1~2초간 그대로 유지해주세요</li>
          <li>인식이 안 되면 화면의 안내 배너를 눌러서 다음 단계로 넘길 수 있어요</li>
        </ul>
      </div>
      <button class="btn btn-secondary" onclick="goTo('screen-home')" style="margin-top:14px;">확인했습니다</button>
    </div>
  </div>

  <!-- CAMERA -->
  <div class="screen" id="screen-camera">
    <div class="topbar">
      <button class="back" onclick="stopCameraAndGo('screen-home')">← 홈</button>
      <div class="counter" id="shotCounter">0 / 4</div>
    </div>
    <p style="text-align:center; font-size:12.5px; color:#c77; margin:0 0 8px; line-height:1.5;">
      💡 밝은 곳 · 손을 화면 중앙에 크게 · 30~80cm 거리 · 동작 1~2초 유지
    </p>
    <div class="cam-wrap">
      <video id="video" autoplay playsinline muted></video>
      <canvas id="overlayCanvas" class="overlay-canvas"></canvas>
      <div class="overlay-emoji" id="overlayEmoji">🫶</div>
      <div class="gesture-tag" id="gestureTag" style="display:none;"></div>
      <div class="countdown" id="countdown">3</div>
      <div class="flash" id="flash"></div>
    </div>
    <p class="loading-msg" id="loadingMsg">손동작 인식을 준비하고 있어요…</p>
    <div class="target-banner" id="targetBanner" style="display:none;">
      <div id="targetText">👍 응원해요 동작을 보여주세요</div>
      <div class="hold-bar"><div class="hold-bar-fill" id="holdBarFill"></div></div>
    </div>
    <div class="shots-strip" id="shotsStrip">
      <div class="slot"></div><div class="slot"></div><div class="slot"></div><div class="slot"></div>
    </div>
    <div class="gesture-legend">
      <div class="g" id="leg-0"><span>👍</span>응원해요</div>
      <div class="g" id="leg-1"><span>✌️</span>우리반최고</div>
      <div class="g" id="leg-2"><span>🤟</span>사랑해요</div>
      <div class="g" id="leg-3"><span>✋</span>함께해요</div>
    </div>
    <div class="shoot-row">
      <button class="btn btn-secondary" onclick="resetShots()">다시 찍기</button>
      <button class="btn btn-primary" id="shootBtn" onclick="startBurst()">📷 촬영 시작</button>
    </div>
  </div>

  <!-- RESULT -->
  <div class="screen" id="screen-result">
    <div class="topbar">
      <button class="back" onclick="goTo('screen-home')">← 홈</button>
      <div class="counter">완성!</div>
    </div>
    <div id="resultCanvasWrap"><canvas id="finalCanvas" width="800" height="800"></canvas></div>
    <div class="result-actions">
      <button class="btn btn-secondary" onclick="retakeFromResult()">다시 찍기</button>
      <button class="btn btn-primary" id="downloadBtn">⬇️ 저장하기</button>
    </div>
  </div>

</div>

<script type="module">
import { GestureRecognizer, FilesetResolver } from "https://cdn.jsdelivr.net/npm/@mediapipe/tasks-vision@0.10.14/vision_bundle.mjs";

const GESTURE_SEQUENCE = [
  { name: "Thumb_Up",  emoji: "👍", label: "응원해요",   minScore: 0.75 },
  { name: "Victory",   emoji: "✌️", label: "우리반 최고", minScore: 0.75 },
  { name: "ILoveYou",  emoji: "🤟", label: "사랑해요",   minScore: 0.7 },
  { name: "Open_Palm", emoji: "✋", label: "함께해요",   minScore: 0.6 }
];
const REQUIRED_HOLD_FRAMES = 10; // 약 0.3~0.5초 이상 안정적으로 유지해야 인식

let video = document.getElementById('video');
let overlayCanvas = document.getElementById('overlayCanvas');
let stream = null;
let gestureRecognizer = null;
let recognizing = false;
let shots = [];
let currentEmoji = "🫶";
let capturing = false;

// gesture-lock state
let targetStep = null;
let holdFrames = 0;
let waitingResolve = null;

async function initRecognizer(){
  try{
    const vision = await FilesetResolver.forVisionTasks(
      "https://cdn.jsdelivr.net/npm/@mediapipe/tasks-vision@0.10.14/wasm"
    );
    gestureRecognizer = await GestureRecognizer.createFromOptions(vision, {
      baseOptions: {
        modelAssetPath: "https://storage.googleapis.com/mediapipe-models/gesture_recognizer/gesture_recognizer/float16/1/gesture_recognizer.task"
      },
      runningMode: "VIDEO",
      numHands: 1
    });
    document.getElementById('loadingMsg').style.display = 'none';
  }catch(err){
    document.getElementById('loadingMsg').textContent = '손동작 인식 로딩에 실패했어요. 촬영은 그대로 가능합니다.';
  }
}
initRecognizer();

function goTo(id){
  document.querySelectorAll('.screen').forEach(s=>s.classList.remove('active'));
  document.getElementById(id).classList.add('active');
  if(id === 'screen-camera'){ startCamera(); }
}

function stopCameraAndGo(id){
  recognizing = false;
  if(stream){ stream.getTracks().forEach(t=>t.stop()); stream = null; }
  goTo(id);
}

async function startCamera(){
  if(stream) return;
  try{
    stream = await navigator.mediaDevices.getUserMedia({ video: { facingMode: 'user' }, audio:false });
    video.srcObject = stream;
    video.addEventListener('loadeddata', () => {
      overlayCanvas.width = video.videoWidth;
      overlayCanvas.height = video.videoHeight;
      recognizing = true;
      requestAnimationFrame(recognizeLoop);
    }, { once:true });
  }catch(err){
    alert('카메라를 사용할 수 없습니다. 브라우저 권한을 확인해주세요.\n(' + err.message + ')');
  }
}

function recognizeLoop(){
  if(!recognizing) return;
  if(gestureRecognizer && video.readyState >= 2){
    const result = gestureRecognizer.recognizeForVideo(video, performance.now());
    const tag = document.getElementById('gestureTag');
    let topName = null, topScore = 0;
    if(result.gestures && result.gestures.length > 0){
      const top = result.gestures[0][0];
      topName = top.categoryName;
      topScore = top.score;
      const known = GESTURE_SEQUENCE.find(g=>g.name===topName);
      if(known && topScore > 0.5){
        tag.style.display = 'block';
        tag.textContent = known.emoji + ' ' + known.label;
      } else {
        tag.style.display = 'none';
      }
    } else {
      tag.style.display = 'none';
    }

    // gesture-lock progress (only while waiting for a specific target gesture)
    if(targetStep){
      if(topName === targetStep.name && topScore >= targetStep.minScore){
        holdFrames++;
      } else {
        holdFrames = Math.max(0, holdFrames - 2); // 흔들리면 빠르게 감소
      }
      const pct = Math.min(100, Math.round((holdFrames / REQUIRED_HOLD_FRAMES) * 100));
      const fill = document.getElementById('holdBarFill');
      if(fill) fill.style.width = pct + '%';

      if(holdFrames >= REQUIRED_HOLD_FRAMES){
        holdFrames = 0;
        targetStep = null;
        if(waitingResolve){
          const r = waitingResolve;
          waitingResolve = null;
          r();
        }
      }
    }
  }
  requestAnimationFrame(recognizeLoop);
}

function showEmojiPop(emoji){
  const overlay = document.getElementById('overlayEmoji');
  overlay.textContent = emoji;
  overlay.classList.remove('show');
  requestAnimationFrame(()=>overlay.classList.add('show'));
  setTimeout(()=>overlay.classList.remove('show'), 1200);
}

function waitForGesture(step, stepIndex){
  const banner = document.getElementById('targetBanner');
  const targetText = document.getElementById('targetText');
  const fill = document.getElementById('holdBarFill');
  document.querySelectorAll('.gesture-legend .g').forEach(el=>el.classList.remove('active'));
  const legEl = document.getElementById('leg-'+stepIndex);
  if(legEl) legEl.classList.add('active');

  banner.style.display = 'block';
  targetText.textContent = step.emoji + ' ' + step.label + ' 동작을 보여주세요 (' + (stepIndex+1) + '/4) · 안 되면 여기를 탭';
  fill.style.width = '0%';
  holdFrames = 0;
  targetStep = step;

  return new Promise(resolve=>{
    let timeoutId = null;
    function finish(){
      if(timeoutId) clearTimeout(timeoutId);
      banner.onclick = null;
      targetStep = null;
      holdFrames = 0;
      waitingResolve = null;
      resolve();
    }
    waitingResolve = finish;
    banner.onclick = () => { finish(); }; // 인식이 안 되면 눌러서 강제로 다음 단계
    timeoutId = setTimeout(() => { finish(); }, 15000); // 15초 넘으면 자동 진행
  });
}

function resetShots(){
  shots = [];
  document.querySelectorAll('#shotsStrip .slot').forEach(s=>s.innerHTML='');
  document.getElementById('shotCounter').textContent = '0 / 4';
  document.getElementById('targetBanner').style.display = 'none';
  document.getElementById('targetBanner').onclick = null;
  document.querySelectorAll('.gesture-legend .g').forEach(el=>el.classList.remove('active'));
  targetStep = null;
  holdFrames = 0;
  if(waitingResolve){
    const r = waitingResolve;
    waitingResolve = null;
    r(); // 진행 중이던 대기를 확실히 끝내서 이전 루프가 영원히 멈춰있지 않도록 함
  }
  capturing = false;
  document.getElementById('shootBtn').disabled = false;
}

async function startBurst(){
  if(capturing) return;
  if(shots.length >= 4){ resetShots(); }
  capturing = true;
  document.getElementById('shootBtn').disabled = true;

  for(let i=0; i<4; i++){
    const step = GESTURE_SEQUENCE[i];
    await waitForGesture(step, i);          // 해당 동작을 실제로 취할 때까지 대기
    showEmojiPop(step.emoji);
    currentEmoji = step.emoji;
    await countdownAndCapture();            // 5초 카운트다운 후 촬영
  }

  document.getElementById('targetBanner').style.display = 'none';
  document.querySelectorAll('.gesture-legend .g').forEach(el=>el.classList.remove('active'));
  capturing = false;
  document.getElementById('shootBtn').disabled = false;
  buildFinalImage();
  goTo('screen-result');
}

function countdownAndCapture(){
  return new Promise(resolve=>{
    const cd = document.getElementById('countdown');
    let n = 5;
    cd.textContent = n;
    cd.classList.add('show');
    const interval = setInterval(()=>{
      n--;
      if(n>0){ cd.textContent = n; }
      else{
        clearInterval(interval);
        cd.classList.remove('show');
        capturePhoto();
        resolve();
      }
    }, 700);
  });
}

function capturePhoto(){
  const flash = document.getElementById('flash');
  flash.classList.add('on');
  setTimeout(()=>flash.classList.remove('on'), 120);

  const canvas = document.createElement('canvas');
  const w = video.videoWidth || 640;
  const h = video.videoHeight || 480;
  canvas.width = w; canvas.height = h;
  const ctx = canvas.getContext('2d');
  ctx.translate(w,0); ctx.scale(-1,1);
  ctx.drawImage(video, 0, 0, w, h);
  ctx.scale(-1,1); ctx.translate(-w,0);
  ctx.font = Math.floor(h*0.14) + 'px sans-serif';
  ctx.textAlign = 'center';
  ctx.fillText(currentEmoji, w/2, h*0.2);

  const dataUrl = canvas.toDataURL('image/png');
  shots.push(dataUrl);
  const slot = document.querySelectorAll('#shotsStrip .slot')[shots.length-1];
  if(slot){ slot.innerHTML = '<img src="'+dataUrl+'">'; }
  document.getElementById('shotCounter').textContent = shots.length + ' / 4';
}

function buildFinalImage(){
  const canvas = document.getElementById('finalCanvas');
  const ctx = canvas.getContext('2d');
  const W = canvas.width, H = canvas.height;
  const pad = 24, gap = 16, headerH = 96, footerH = 70;
  const gridW = W - pad*2;
  const gridH = H - headerH - footerH - pad*2;
  const cellW = (gridW - gap) / 2;
  const cellH = (gridH - gap) / 2;

  ctx.fillStyle = '#ffffff'; ctx.fillRect(0,0,W,H);
  ctx.fillStyle = '#ff6b81'; ctx.fillRect(0,0,W,10);

  ctx.fillStyle = '#ff6b81';
  ctx.font = 'bold 38px sans-serif';
  ctx.textAlign = 'center';
  ctx.fillText('🌸 공감 네컷', W/2, headerH*0.78);

  const positions = [
    [pad, headerH+pad],
    [pad+cellW+gap, headerH+pad],
    [pad, headerH+pad+cellH+gap],
    [pad+cellW+gap, headerH+pad+cellH+gap]
  ];

  const imgs = shots.map(src=>{ const img = new Image(); img.src = src; return img; });
  let loaded = 0;

  function drawAll(){
    imgs.forEach((img,i)=>{
      const [x,y] = positions[i];
      ctx.save();
      ctx.beginPath();
      if(ctx.roundRect) ctx.roundRect(x,y,cellW,cellH,14); else ctx.rect(x,y,cellW,cellH);
      ctx.clip();
      const ir = img.width/img.height, cr = cellW/cellH;
      let dw, dh, dx, dy;
      if(ir > cr){ dh = cellH; dw = dh*ir; dx = x-(dw-cellW)/2; dy = y; }
      else { dw = cellW; dh = dw/ir; dx = x; dy = y-(dh-cellH)/2; }
      ctx.drawImage(img, dx, dy, dw, dh);
      ctx.restore();
      ctx.strokeStyle = '#ffffff'; ctx.lineWidth = 4;
      ctx.strokeRect(x,y,cellW,cellH);
    });

    ctx.fillStyle = '#9a94a8';
    ctx.font = '18px sans-serif';
    ctx.textAlign = 'left';
    const dateStr = new Date().toLocaleDateString('ko-KR');
    ctx.fillText('양구초등학교 · 우리 반 공감 순간 · ' + dateStr, pad, H-footerH*0.55);
    ctx.textAlign = 'center';

    const mascot = new Image();
    mascot.onload = ()=>{
      const mh = footerH*0.9;
      const mw = mh * (mascot.width/mascot.height);
      ctx.drawImage(mascot, W - pad - mw, H - footerH*1.05, mw, mh);
      finalizeDownload(canvas);
    };
    mascot.onerror = ()=> finalizeDownload(canvas);
    mascot.src = 'assets/mascot.png';
  }

  function finalizeDownload(canvas){
    document.getElementById('downloadBtn').onclick = ()=>{
      const a = document.createElement('a');
      a.download = 'gonggam-necut-' + Date.now() + '.png';
      a.href = canvas.toDataURL('image/png');
      a.click();
    };
  }

  imgs.forEach(img=>{
    img.onload = ()=>{ loaded++; if(loaded === imgs.length) drawAll(); };
  });
}

function retakeFromResult(){ resetShots(); goTo('screen-camera'); }

window.goTo = goTo;
window.stopCameraAndGo = stopCameraAndGo;
window.resetShots = resetShots;
window.startBurst = startBurst;
window.retakeFromResult = retakeFromResult;
</script>
</body>
</html>
