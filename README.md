<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no" />
  <meta name="theme-color" content="#07120e" />
  <title>حكم الشوارع</title>
  <style>
    :root {
      --bg: #07120e;
      --panel: rgba(12, 27, 22, 0.88);
      --border: rgba(255, 255, 255, 0.14);
      --text: #f3f7f5;
      --muted: #9eb3a9;
      --red: #ef4444;
      --blue: #3b82f6;
      --accent: #10b981;
      --warn: #f59e0b;
    }

    * {
      box-sizing: border-box;
      -webkit-tap-highlight-color: transparent;
      user-select: none;
    }

    html, body {
      margin: 0;
      padding: 0;
      width: 100%;
      height: 100%;
      background: var(--bg);
      color: var(--text);
      font-family: system-ui, -apple-system, "Segoe UI", Tahoma, sans-serif;
      overflow: hidden;
    }

    .app {
      position: relative;
      width: 100vw;
      height: 100vh;
      display: flex;
      flex-direction: column;
      justify-content: space-between;
      background: radial-gradient(circle at center, #102a21 0%, #07120e 75%);
    }

    video, canvas#overlay {
      position: fixed;
      inset: 0;
      width: 100%;
      height: 100%;
      object-fit: cover;
      z-index: 0;
    }

    canvas#overlay {
      pointer-events: none;
      z-index: 1;
    }

    .top-bar, .bottom-bar, .alert-banner {
      position: relative;
      z-index: 3;
    }

    .top-bar {
      display: flex;
      align-items: center;
      justify-content: space-between;
      gap: 8px;
      padding: 10px 12px;
      background: linear-gradient(to bottom, rgba(0,0,0,0.78), transparent);
    }

    .brand {
      display: flex;
      align-items: center;
      gap: 8px;
      background: var(--panel);
      border: 1px solid var(--border);
      padding: 6px 12px;
      border-radius: 999px;
      font-weight: 800;
      font-size: 14px;
      backdrop-filter: blur(8px);
    }

    .dot {
      width: 9px;
      height: 9px;
      border-radius: 50%;
      background: var(--accent);
      box-shadow: 0 0 10px var(--accent);
    }

    .scoreboard {
      display: flex;
      align-items: center;
      gap: 8px;
      background: var(--panel);
      border: 1px solid var(--border);
      padding: 5px 12px;
      border-radius: 999px;
      backdrop-filter: blur(8px);
      font-weight: 800;
    }

    .team-score {
      display: flex;
      align-items: center;
      gap: 6px;
      font-size: 15px;
    }

    .badge-red {
      background: var(--red);
      color: #fff;
      padding: 2px 8px;
      border-radius: 999px;
      font-size: 12px;
    }

    .badge-blue {
      background: var(--blue);
      color: #fff;
      padding: 2px 8px;
      border-radius: 999px;
      font-size: 12px;
    }

    .top-actions {
      display: flex;
      align-items: center;
      gap: 6px;
    }

    button {
      border: 1px solid var(--border);
      background: var(--panel);
      color: var(--text);
      border-radius: 12px;
      padding: 8px 11px;
      font-size: 13px;
      font-weight: 700;
      cursor: pointer;
      backdrop-filter: blur(8px);
      transition: transform 0.08s ease, opacity 0.15s ease;
    }

    button:active {
      transform: scale(0.96);
    }

    .alert-banner {
      margin: 0 auto;
      padding: 10px 20px;
      border-radius: 16px;
      background: rgba(16, 185, 129, 0.92);
      color: #04130d;
      font-size: 20px;
      font-weight: 900;
      letter-spacing: 0.2px;
      box-shadow: 0 10px 30px rgba(0,0,0,0.45);
      opacity: 0;
      transform: translateY(-8px) scale(0.96);
      transition: opacity 0.14s ease, transform 0.14s ease;
      pointer-events: none;
      text-align: center;
    }

    .alert-banner.show {
      opacity: 1;
      transform: translateY(0) scale(1);
    }

    .alert-banner.danger {
      background: rgba(239, 68, 68, 0.95);
      color: #fff;
    }

    .alert-banner.warn {
      background: rgba(245, 158, 11, 0.95);
      color: #160e00;
    }

    .alert-banner.info {
      background: rgba(59, 130, 246, 0.95);
      color: #fff;
    }

    .bottom-bar {
      display: flex;
      flex-direction: column;
      gap: 8px;
      padding: 10px 12px 14px;
      background: linear-gradient(to top, rgba(0,0,0,0.84), transparent);
    }

    .calls-grid {
      display: grid;
      grid-template-columns: repeat(3, 1fr);
      gap: 8px;
    }

    .call-btn {
      padding: 12px 8px;
      font-size: 14px;
      font-weight: 800;
      border-radius: 14px;
      text-align: center;
    }

    .btn-goal-red { background: rgba(239, 68, 68, 0.28); border-color: rgba(239, 68, 68, 0.7); }
    .btn-goal-blue { background: rgba(59, 130, 246, 0.28); border-color: rgba(59, 130, 246, 0.7); }
    .btn-foul { background: rgba(245, 158, 11, 0.25); border-color: rgba(245, 158, 11, 0.65); }
    .btn-hand { background: rgba(168, 85, 247, 0.26); border-color: rgba(168, 85, 247, 0.65); }
    .btn-pen { background: rgba(16, 185, 129, 0.26); border-color: rgba(16, 185, 129, 0.65); }
    .btn-throw { background: rgba(14, 165, 233, 0.26); border-color: rgba(14, 165, 233, 0.65); }

    .sub-controls {
      display: flex;
      align-items: center;
      justify-content: space-between;
      gap: 8px;
      font-size: 12px;
      color: var(--muted);
    }

    .sub-left, .sub-right {
      display: flex;
      align-items: center;
      gap: 6px;
    }

    input[type="file"] {
      display: none;
    }

    .flash {
      position: fixed;
      inset: 0;
      z-index: 2;
      pointer-events: none;
      opacity: 0;
      background: rgba(16, 185, 129, 0.22);
      transition: opacity 0.15s ease;
    }

    .flash.active {
      opacity: 1;
    }
  </style>
</head>
<body>
  <div class="app" id="app">
    <video id="video" playsinline muted autoplay></video>
    <canvas id="overlay"></canvas>
    <div class="flash" id="flash"></div>

    <div class="top-bar">
      <div class="brand">
        <span class="dot" id="statusDot"></span>
        <span>حكم الشوارع</span>
      </div>

      <div class="scoreboard">
        <div class="team-score">
          <span class="badge-red">الأحمر</span>
          <span id="scoreRed">0</span>
        </div>
        <span>-</span>
        <div class="team-score">
          <span id="scoreBlue">0</span>
          <span class="badge-blue">الأزرق</span>
        </div>
      </div>

      <div class="top-actions">
        <button id="btnFullscreen" title="ملء الشاشة">ملء الشاشة</button>
        <button id="btnReset" title="تصفير النتيجة">تصفير</button>
      </div>
    </div>

    <div class="alert-banner" id="alertBanner">قرار فوري</div>

    <div class="bottom-bar">
      <div class="calls-grid">
        <button class="call-btn btn-goal-red" onclick="triggerCall('هدف للأحمر!', 'danger', 'red')">هدف أحمر</button>
        <button class="call-btn btn-goal-blue" onclick="triggerCall('هدف للأزرق!', 'info', 'blue')">هدف أزرق</button>
        <button class="call-btn btn-foul" onclick="triggerCall('خطأ (فاول)!', 'warn')">فاول</button>
        <button class="call-btn btn-hand" onclick="triggerCall('لمسة يد!', 'warn')">لمسة يد</button>
        <button class="call-btn btn-pen" onclick="triggerCall('ركلة جزاء!', 'danger')">ركلة جزاء</button>
        <button class="call-btn btn-throw" onclick="triggerCall('رمية تماس!', 'info')">رمية تماس</button>
      </div>

      <div class="sub-controls">
        <div class="sub-left">
          <button id="btnCamera">تشغيل الكاميرا</button>
          <button id="btnSwitch">تبديل الكاميرا</button>
          <label for="videoFile" class="call-btn" style="padding: 8px 10px; cursor: pointer;">فيديو</label>
          <input type="file" id="videoFile" accept="video/*" />
        </div>
        <div class="sub-right">
          <span id="motionStatus">رصد فوري نشط</span>
        </div>
      </div>
    </div>
  </div>

  <script>
    const video = document.getElementById('video');
    const overlay = document.getElementById('overlay');
    const ctx = overlay.getContext('2d');
    const alertBanner = document.getElementById('alertBanner');
    const flash = document.getElementById('flash');
    const scoreRedEl = document.getElementById('scoreRed');
    const scoreBlueEl = document.getElementById('scoreBlue');
    const motionStatus = document.getElementById('motionStatus');

    let scoreRed = 0;
    let scoreBlue = 0;
    let facingMode = 'environment';
    let currentStream = null;
    let lastFrame = null;
    let cooldown = 0;
    let alertTimer = null;

    const sampleCanvas = document.createElement('canvas');
    sampleCanvas.width = 160;
    sampleCanvas.height = 90;
    const sCtx = sampleCanvas.getContext('2d', { willReadFrequently: true });

    function beep(freq = 880, duration = 0.14) {
      try {
        const AudioCtx = window.AudioContext || window.webkitAudioContext;
        const audioCtx = new AudioCtx();
        const osc = audioCtx.createOscillator();
        const gain = audioCtx.createGain();
        osc.type = 'triangle';
        osc.frequency.value = freq;
        gain.gain.value = 0.16;
        osc.connect(gain);
        gain.connect(audioCtx.destination);
        osc.start();
        osc.stop(audioCtx.currentTime + duration);
      } catch (_) {}
    }

    function speak(text) {
      try {
        if ('speechSynthesis' in window) {
          window.speechSynthesis.cancel();
          const utter = new SpeechSynthesisUtterance(text);
          utter.lang = 'ar-SA';
          utter.rate = 1.15;
          window.speechSynthesis.speak(utter);
        }
      } catch (_) {}
    }

    function triggerCall(message, tone = 'info', goalTeam = null) {
      if (goalTeam === 'red') {
        scoreRed += 1;
        scoreRedEl.textContent = scoreRed;
      } else if (goalTeam === 'blue') {
        scoreBlue += 1;
        scoreBlueEl.textContent = scoreBlue;
      }

      alertBanner.textContent = message;
      alertBanner.className = `alert-banner show ${tone}`;
      flash.classList.add('active');
      beep(goalTeam ? 1046 : 740, 0.16);
      speak(message);

      clearTimeout(alertTimer);
      alertTimer = setTimeout(() => {
        alertBanner.classList.remove('show');
        flash.classList.remove('active');
      }, 1500);
    }

    async function startCamera() {
      stopMedia();
      try {
        currentStream = await navigator.mediaDevices.getUserMedia({
          video: { facingMode },
          audio: false
        });
        video.srcObject = currentStream;
        await video.play();
        motionStatus.textContent = 'الكاميرا تعمل — رصد لحظي';
      } catch (err) {
        motionStatus.textContent = 'الكاميرا غير متاحة محليًا — استخدم الأزرار أو الفيديو';
      }
    }

    function stopMedia() {
      if (currentStream) {
        currentStream.getTracks().forEach(t => t.stop());
        currentStream = null;
      }
      video.srcObject = null;
    }

    function resizeOverlay() {
      overlay.width = window.innerWidth;
      overlay.height = window.innerHeight;
      ctx.clearRect(0, 0, overlay.width, overlay.height);
      ctx.strokeStyle = 'rgba(16, 185, 129, 0.45)';
      ctx.lineWidth = 2;
      const pad = 24;
      ctx.strokeRect(pad, pad + 44, overlay.width - pad * 2, overlay.height - pad * 2 - 110);
    }

    function detectFirstLookMotion() {
      if (cooldown > 0) cooldown--;

      if ((video.readyState >= 2) && (video.srcObject || !video.paused)) {
        sCtx.drawImage(video, 0, 0, sampleCanvas.width, sampleCanvas.height);
        const frame = sCtx.getImageData(0, 0, sampleCanvas.width, sampleCanvas.height).data;

        if (lastFrame) {
          let changedPixels = 0;
          let leftMotion = 0;
          let rightMotion = 0;
          const halfW = sampleCanvas.width / 2;

          for (let y = 12; y < sampleCanvas.height - 12; y += 2) {
            for (let x = 8; x < sampleCanvas.width - 8; x += 2) {
              const i = (y * sampleCanvas.width + x) * 4;
              const diff =
                Math.abs(frame[i] - lastFrame[i]) +
                Math.abs(frame[i + 1] - lastFrame[i + 1]) +
                Math.abs(frame[i + 2] - lastFrame[i + 2]);

              if (diff > 95) {
                changedPixels++;
                if (x < halfW) leftMotion++;
                else rightMotion++;
              }
            }
          }

          if (changedPixels > 240 && cooldown === 0) {
            cooldown = 32;
            if (leftMotion > rightMotion * 1.45) {
              triggerCall('رصد حركة سريعة جهة اليسار — رمية تماس / هجمة', 'info');
            } else if (rightMotion > leftMotion * 1.45) {
              triggerCall('رصد حركة سريعة جهة اليمين — رمية تماس / هجمة', 'info');
            } else {
              triggerCall('رصد التحام سريع في اللعب!', 'warn');
            }
          }
        }
        lastFrame = new Uint8ClampedArray(frame);
      }

      requestAnimationFrame(detectFirstLookMotion);
    }

    document.getElementById('btnCamera').addEventListener('click', startCamera);

    document.getElementById('btnSwitch').addEventListener('click', () => {
      facingMode = facingMode === 'environment' ? 'user' : 'environment';
      startCamera();
    });

    document.getElementById('videoFile').addEventListener('change', (e) => {
      const file = e.target.files?.[0];
      if (!file) return;
      stopMedia();
      video.src = URL.createObjectURL(file);
      video.play();
      motionStatus.textContent = 'تشغيل فيديو للمعاينة';
    });

    document.getElementById('btnReset').addEventListener('click', () => {
      scoreRed = 0;
      scoreBlue = 0;
      scoreRedEl.textContent = '0';
      scoreBlueEl.textContent = '0';
      triggerCall('تم تصفير النتيجة', 'info');
    });

    document.getElementById('btnFullscreen').addEventListener('click', async () => {
      try {
        if (!document.fullscreenElement) {
          await document.documentElement.requestFullscreen();
        } else {
          await document.exitFullscreen();
        }
      } catch (_) {}
    });

    window.addEventListener('resize', resizeOverlay);
    resizeOverlay();
    detectFirstLookMotion();
  </script>
</body>
</html>
