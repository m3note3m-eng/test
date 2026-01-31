<html lang="ru">
<head>
  <meta charset="UTF-8">
  <title>Приватный аудио плеер</title>
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <style>
    body {
      font-family: sans-serif;
      background-color: var(--tg-theme-bg-color, #fff);
      color: var(--tg-theme-text-color, #000);
      margin: 0;
      padding: 20px;
      text-align: center;
    }
    .player {
      display: flex;
      flex-direction: column;
      align-items: center;
      gap: 15px;
    }
    button {
      padding: 10px 20px;
      border: none;
      border-radius: 6px;
      cursor: pointer;
      background-color: var(--tg-theme-button-color, #0088cc);
      color: var(--tg-theme-button-text-color, #fff);
      font-size: 16px;
    }
    input[type="range"] {
      width: 200px;
    }
    canvas {
      width: 100%;
      height: 120px;
      background: rgba(0,0,0,0.1);
      border-radius: 8px;
    }
    .hidden {
      display: none;
    }
    .mini-panel {
      position: fixed;
      bottom: 10px;
      left: 50%;
      transform: translateX(-50%);
      background: var(--tg-theme-bg-color, #fff);
      border: 1px solid rgba(0,0,0,0.2);
      border-radius: 8px;
      padding: 10px;
      display: flex;
      gap: 10px;
      align-items: center;
      box-shadow: 0 2px 6px rgba(0,0,0,0.2);
    }
  </style>
</head>
<body>
  <div class="player" id="playerUI">
    <h2>🎵 Приватный аудио плеер</h2>
    <audio id="audio" src="https://example.com/stream.mp3" preload="none"></audio>
    
    <button id="playBtn">▶️ Воспроизвести</button>
    <input id="volume" type="range" min="0" max="1" step="0.05" value="1">
    <button id="minimizeBtn">📥 Свернуть в фон</button>
    <button id="closeBtn">❌ Закрыть</button>
    <p id="status"></p>
    <canvas id="visualizer"></canvas>
  </div>

  <!-- Мини‑панель управления -->
  <div class="mini-panel hidden" id="miniPanel">
    <button id="miniPlay">▶️</button>
    <input id="miniVolume" type="range" min="0" max="1" step="0.05" value="1">
    <button id="restoreBtn">🔼 Развернуть</button>
  </div>

  <script src="https://telegram.org/js/telegram-web-app.js"></script>
  <script>
    const audio = document.getElementById('audio');
    const playBtn = document.getElementById('playBtn');
    const volume = document.getElementById('volume');
    const closeBtn = document.getElementById('closeBtn');
    const minimizeBtn = document.getElementById('minimizeBtn');
    const status = document.getElementById('status');
    const canvas = document.getElementById('visualizer');
    const ctx = canvas.getContext('2d');
    const playerUI = document.getElementById('playerUI');
    const miniPanel = document.getElementById('miniPanel');
    const miniPlay = document.getElementById('miniPlay');
    const miniVolume = document.getElementById('miniVolume');
    const restoreBtn = document.getElementById('restoreBtn');

    let reconnectTimer = null;
    let audioCtx, analyser, source, dataArray;

    // Управление воспроизведением
    function togglePlay() {
      if (audio.paused) {
        audio.play().then(() => initVisualizer()).catch(err => {
          status.textContent = "Ошибка воспроизведения: " + err.message;
        });
        playBtn.textContent = "⏸️ Пауза";
        miniPlay.textContent = "⏸️";
      } else {
        audio.pause();
        playBtn.textContent = "▶️ Воспроизвести";
        miniPlay.textContent = "▶️";
      }
    }

    playBtn.addEventListener('click', togglePlay);
    miniPlay.addEventListener('click', togglePlay);

    // Громкость
    volume.addEventListener('input', () => audio.volume = volume.value);
    miniVolume.addEventListener('input', () => audio.volume = miniVolume.value);

    // Авто‑переподключение
    function scheduleReconnect() {
      if (reconnectTimer) return;
      status.textContent = "🔄 Переподключение...";
      reconnectTimer = setTimeout(() => {
        reconnectTimer = null;
        audio.load();
        audio.play().catch(() => scheduleReconnect());
      }, 3000);
    }

    audio.addEventListener('error', scheduleReconnect);
    audio.addEventListener('stalled', scheduleReconnect);

    audio.addEventListener('playing', () => {
      status.textContent = "▶️ Воспроизведение идёт";
      if (reconnectTimer) {
        clearTimeout(reconnectTimer);
        reconnectTimer = null;
      }
    });

    // Визуализатор
    function initVisualizer() {
      if (!audioCtx) {
        audioCtx = new (window.AudioContext || window.webkitAudioContext)();
        analyser = audioCtx.createAnalyser();
        source = audioCtx.createMediaElementSource(audio);
        source.connect(analyser);
        analyser.connect(audioCtx.destination);
        analyser.fftSize = 256;
        const bufferLength = analyser.frequencyBinCount;
        dataArray = new Uint8Array(bufferLength);
        draw();
      }
    }

    function draw() {
      requestAnimationFrame(draw);
      analyser.getByteFrequencyData(dataArray);
      ctx.clearRect(0, 0, canvas.width, canvas.height);

      const barWidth = (canvas.width / dataArray.length) * 2.5;
      let x = 0;

      for (let i = 0; i < dataArray.length; i++) {
        const barHeight = dataArray[i] / 2;
        ctx.fillStyle = `rgb(${barHeight+100},50,150)`;
        ctx.fillRect(x, canvas.height - barHeight, barWidth, barHeight);
        x += barWidth + 1;
      }
    }

    // Telegram Mini App интеграция
    Telegram.WebApp.ready();
    Telegram.WebApp.expand();

    closeBtn.addEventListener('click', () => Telegram.WebApp.close());

    minimizeBtn.addEventListener('click', () => {
      playerUI.classList.add('hidden');
      miniPanel.classList.remove('hidden');
      status.textContent = "▶️ Поток играет в фоне";
    });

    restoreBtn.addEventListener('click', () => {
      playerUI.classList.remove('hidden');
      miniPanel.classList.add('hidden');
    });

    document.body.style.backgroundColor = Telegram.WebApp.backgroundColor;
    document.body.style.color = Telegram.WebApp.textColor;
  </script>
</body>
</html>
