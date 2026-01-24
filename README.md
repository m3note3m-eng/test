<html lang="ru">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1,maximum-scale=1,user-scalable=no" />
  <title>Аудиопоток</title>
  <script src="https://telegram.org/js/telegram-web-app.js"></script>
  <style>
    * {
      margin: 0;
      padding: 0;
      box-sizing: border-box;
    }
    
    :root {
      --bg-primary: rgba(18, 18, 18, 0.85);
      --bg-secondary: rgba(255, 255, 255, 0.08);
      --bg-glass: rgba(255, 255, 255, 0.1);
      --text-primary: #ffffff;
      --text-secondary: rgba(255, 255, 255, 0.7);
      --accent: #2ea6ff;
      --accent-hover: #1d8bd9;
      --border-glass: rgba(255, 255, 255, 0.15);
      --shadow-glass: 0 8px 32px rgba(0, 0, 0, 0.2);
      --blur: 20px;
    }
    
    body {
      font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, 'Helvetica Neue', Arial, sans-serif;
      background: linear-gradient(135deg, #0f0f23 0%, #1a1a2e 50%, #16213e 100%);
      color: var(--text-primary);
      min-height: 100vh;
      padding: env(safe-area-inset-top) env(safe-area-inset-right) env(safe-area-inset-bottom) env(safe-area-inset-left);
      display: flex;
      flex-direction: column;
    }
    
    .container {
      max-width: 100%;
      padding: 20px 16px 90px;
      display: flex;
      flex-direction: column;
      gap: 20px;
      flex: 1;
    }
    
    /* Стиль жидкого стекла */
    .glass {
      background: var(--bg-glass);
      backdrop-filter: blur(var(--blur));
      -webkit-backdrop-filter: blur(var(--blur));
      border: 1px solid var(--border-glass);
      border-radius: 24px;
      box-shadow: var(--shadow-glass);
    }
    
    .glass-light {
      background: rgba(255, 255, 255, 0.05);
    }
    
    .header {
      padding: 24px 20px;
      text-align: center;
    }
    
    .title {
      font-size: 24px;
      font-weight: 700;
      margin-bottom: 8px;
      background: linear-gradient(90deg, #2ea6ff, #7c4dff);
      -webkit-background-clip: text;
      -webkit-text-fill-color: transparent;
      background-clip: text;
    }
    
    .subtitle {
      font-size: 14px;
      color: var(--text-secondary);
      line-height: 1.4;
    }
    
    .input-container {
      padding: 20px;
      display: flex;
      flex-direction: column;
      gap: 16px;
    }
    
    .url-input {
      width: 100%;
      padding: 18px 20px;
      background: rgba(0, 0, 0, 0.3);
      border: 1px solid rgba(255, 255, 255, 0.1);
      border-radius: 16px;
      color: var(--text-primary);
      font-size: 16px;
      outline: none;
      transition: all 0.3s ease;
    }
    
    .url-input:focus {
      border-color: var(--accent);
      background: rgba(0, 0, 0, 0.4);
    }
    
    .url-input::placeholder {
      color: var(--text-secondary);
    }
    
    .presets {
      display: flex;
      flex-wrap: wrap;
      gap: 10px;
      margin-top: 8px;
    }
    
    .preset-btn {
      padding: 10px 16px;
      background: rgba(46, 166, 255, 0.1);
      border: 1px solid rgba(46, 166, 255, 0.2);
      border-radius: 12px;
      color: var(--accent);
      font-size: 14px;
      cursor: pointer;
      transition: all 0.3s ease;
      flex: 1;
      min-width: 140px;
    }
    
    .preset-btn:hover {
      background: rgba(46, 166, 255, 0.2);
      transform: translateY(-2px);
    }
    
    .player-container {
      padding: 24px 20px;
      text-align: center;
    }
    
    .player-status {
      font-size: 16px;
      margin-bottom: 20px;
      min-height: 24px;
    }
    
    .player-status.playing {
      color: #4cd137;
    }
    
    .player-status.error {
      color: #ff6b6b;
    }
    
    .player-controls {
      display: grid;
      grid-template-columns: 1fr 1fr;
      gap: 16px;
      margin-bottom: 24px;
    }
    
    .control-btn {
      padding: 20px;
      border: none;
      border-radius: 20px;
      font-size: 16px;
      font-weight: 600;
      cursor: pointer;
      transition: all 0.3s ease;
      display: flex;
      align-items: center;
      justify-content: center;
      gap: 10px;
      min-height: 60px;
    }
    
    .play-btn {
      background: linear-gradient(135deg, #2ea6ff, #1d8bd9);
      color: white;
    }
    
    .stop-btn {
      background: rgba(255, 107, 107, 0.1);
      border: 1px solid rgba(255, 107, 107, 0.2);
      color: #ff6b6b;
    }
    
    .control-btn:active {
      transform: scale(0.98);
    }
    
    .volume-container {
      margin-top: 20px;
    }
    
    .volume-slider {
      width: 100%;
      height: 6px;
      -webkit-appearance: none;
      appearance: none;
      background: rgba(255, 255, 255, 0.1);
      border-radius: 3px;
      outline: none;
    }
    
    .volume-slider::-webkit-slider-thumb {
      -webkit-appearance: none;
      appearance: none;
      width: 24px;
      height: 24px;
      background: var(--accent);
      border-radius: 50%;
      cursor: pointer;
      box-shadow: 0 2px 8px rgba(0, 0, 0, 0.3);
    }
    
    .telegram-player-notice {
      background: rgba(46, 166, 255, 0.1);
      border: 1px solid rgba(46, 166, 255, 0.2);
      border-radius: 16px;
      padding: 16px;
      margin-top: 20px;
      font-size: 14px;
      color: var(--accent);
      display: none;
    }
    
    .telegram-player-notice.show {
      display: block;
    }
    
    .footer {
      position: fixed;
      bottom: 0;
      left: 0;
      right: 0;
      padding: 16px 20px 20px;
      background: linear-gradient(transparent, rgba(0, 0, 0, 0.8));
      backdrop-filter: blur(10px);
      -webkit-backdrop-filter: blur(10px);
      z-index: 100;
    }
    
    .action-bar {
      display: grid;
      grid-template-columns: 1fr 1fr;
      gap: 12px;
    }
    
    .action-btn {
      padding: 18px;
      border: 1px solid rgba(255, 255, 255, 0.1);
      border-radius: 16px;
      background: rgba(255, 255, 255, 0.05);
      color: var(--text-primary);
      font-size: 16px;
      cursor: pointer;
      transition: all 0.3s ease;
    }
    
    .action-btn.primary {
      background: linear-gradient(135deg, #2ea6ff, #7c4dff);
      border: none;
      font-weight: 600;
    }
    
    .action-btn:active {
      transform: scale(0.98);
    }
    
    /* Адаптивность */
    @media (max-width: 480px) {
      .container {
        padding: 16px 12px 100px;
      }
      
      .header {
        padding: 20px 16px;
      }
      
      .title {
        font-size: 22px;
      }
      
      .input-container,
      .player-container {
        padding: 20px 16px;
      }
      
      .presets {
        flex-direction: column;
      }
      
      .preset-btn {
        min-width: 100%;
      }
      
      .control-btn {
        padding: 18px;
        font-size: 15px;
        min-height: 56px;
      }
      
      .action-btn {
        padding: 16px;
        font-size: 15px;
      }
    }
    
    @media (max-width: 360px) {
      .title {
        font-size: 20px;
      }
      
      .control-btn {
        font-size: 14px;
        padding: 16px;
      }
      
      .player-controls {
        gap: 12px;
      }
    }
    
    /* Анимации */
    @keyframes pulse {
      0% { opacity: 1; }
      50% { opacity: 0.6; }
      100% { opacity: 1; }
    }
    
    .loading {
      animation: pulse 1.5s infinite;
    }
    
    /* Эффекты жидкого стекла */
    .glass:hover {
      background: rgba(255, 255, 255, 0.12);
    }
    
    /* Плавные переходы */
    * {
      transition: background-color 0.3s ease, transform 0.2s ease;
    }
  </style>
</head>
<body>
  <div class="container">
    <header class="header glass">
      <h1 class="title">Аудиопоток</h1>
      <p class="subtitle">Вставьте прямую ссылку на аудиопоток или выберите станцию</p>
    </header>
    
    <section class="input-container glass">
      <input 
        type="url" 
        class="url-input" 
        id="urlInput"
        placeholder="https://example.com/stream.mp3"
        inputmode="url"
        autocomplete="off"
      />
      
      <div class="presets">
        <button class="preset-btn glass-light" data-url="https://media2.datacenter.by/stream/stalica/stream">
          🎵 Stalica Radio
        </button>
        <button class="preset-btn glass-light" data-url="https://stream.radiopub.ru/radiorecord_lofi">
          ☕ LoFi Radio
        </button>
        <button class="preset-btn glass-light" data-url="https://icecast.radiofrance.fr/fip-midfi.mp3">
          🇫🇷 FIP Radio
        </button>
      </div>
    </section>
    
    <section class="player-container glass">
      <div class="player-status" id="playerStatus">Готов к воспроизведению</div>
      
      <div class="player-controls">
        <button class="control-btn play-btn glass" id="playPauseBtn">
          <span>▶</span>
          <span>Воспроизвести</span>
        </button>
        <button class="control-btn stop-btn glass" id="stopBtn">
          <span>■</span>
          <span>Стоп</span>
        </button>
      </div>
      
      <div class="volume-container">
        <input 
          type="range" 
          class="volume-slider" 
          id="volumeSlider"
          min="0" 
          max="100" 
          value="80"
        />
      </div>
      
      <div class="telegram-player-notice" id="telegramNotice">
        🎧 Используется Telegram Player для фонового воспроизведения
      </div>
    </section>
  </div>
  
  <footer class="footer">
    <div class="action-bar">
      <button class="action-btn" id="loadBtn">
        📥 Загрузить
      </button>
      <button class="action-btn primary" id="telegramBtn">
        🎵 Telegram Player
      </button>
    </div>
  </footer>

  <script>
    // Инициализация Telegram WebApp
    const tg = window.Telegram?.WebApp;
    let currentTrackId = null;
    let isTelegramPlayer = false;
    let player = null;
    let volume = 80;

    // Элементы интерфейса
    const elements = {
      urlInput: document.getElementById('urlInput'),
      playPauseBtn: document.getElementById('playPauseBtn'),
      stopBtn: document.getElementById('stopBtn'),
      loadBtn: document.getElementById('loadBtn'),
      telegramBtn: document.getElementById('telegramBtn'),
      playerStatus: document.getElementById('playerStatus'),
      volumeSlider: document.getElementById('volumeSlider'),
      telegramNotice: document.getElementById('telegramNotice'),
      presetBtns: document.querySelectorAll('.preset-btn')
    };

    // Инициализация
    function init() {
      if (tg) {
        tg.ready();
        tg.expand();
        tg.MainButton.hide();
        
        // Адаптация под цветовую тему Telegram
        applyTelegramTheme();
        tg.onEvent('themeChanged', applyTelegramTheme);
      }
      
      // Восстановить последний URL
      const lastUrl = localStorage.getItem('stream_url');
      if (lastUrl) {
        elements.urlInput.value = lastUrl;
      }
      
      // Восстановить громкость
      const savedVolume = localStorage.getItem('player_volume');
      if (savedVolume) {
        volume = parseInt(savedVolume);
        elements.volumeSlider.value = volume;
      }
      
      // Создать аудиоэлемент
      player = new Audio();
      player.volume = volume / 100;
      player.preload = 'none';
      
      setupEventListeners();
      updatePlayerStatus('Готов к воспроизведению');
    }

    function applyTelegramTheme() {
      if (!tg || !tg.themeParams) return;
      
      const theme = tg.themeParams;
      document.documentElement.style.setProperty('--accent', theme.link_color || '#2ea6ff');
    }

    function setupEventListeners() {
      // Кнопки управления
      elements.playPauseBtn.addEventListener('click', togglePlayPause);
      elements.stopBtn.addEventListener('click', stopPlayback);
      elements.loadBtn.addEventListener('click', loadStream);
      elements.telegramBtn.addEventListener('click', toggleTelegramPlayer);
      
      // Слайдер громкости
      elements.volumeSlider.addEventListener('input', (e) => {
        volume = e.target.value;
        localStorage.setItem('player_volume', volume);
        if (player) {
          player.volume = volume / 100;
        }
      });
      
      // Пресеты
      elements.presetBtns.forEach(btn => {
        btn.addEventListener('click', () => {
          const url = btn.dataset.url;
          elements.urlInput.value = url;
          loadStream();
        });
      });
      
      // Enter для загрузки
      elements.urlInput.addEventListener('keypress', (e) => {
        if (e.key === 'Enter') loadStream();
      });
      
      // События аудиоплеера
      if (player) {
        player.addEventListener('playing', () => {
          updatePlayerStatus('Идёт воспроизведение', 'playing');
          updatePlayButton(true);
        });
        
        player.addEventListener('pause', () => {
          updatePlayerStatus('Пауза');
          updatePlayButton(false);
        });
        
        player.addEventListener('waiting', () => {
          updatePlayerStatus('Буферизация...', 'loading');
        });
        
        player.addEventListener('error', () => {
          updatePlayerStatus('Ошибка воспроизведения', 'error');
          updatePlayButton(false);
        });
        
        player.addEventListener('ended', () => {
          updatePlayerStatus('Воспроизведение завершено');
          updatePlayButton(false);
        });
      }
    }

    function isValidUrl(url) {
      try {
        const urlObj = new URL(url);
        return ['http:', 'https:'].includes(urlObj.protocol);
      } catch {
        return false;
      }
    }

    function loadStream() {
      const url = elements.urlInput.value.trim();
      
      if (!isValidUrl(url)) {
        updatePlayerStatus('Некорректный URL', 'error');
        tg?.HapticFeedback.notificationOccurred('error');
        return;
      }
      
      // Сохранить URL
      localStorage.setItem('stream_url', url);
      
      if (isTelegramPlayer && tg?.openAudio) {
        openInTelegramPlayer(url);
      } else {
        openInNativePlayer(url);
      }
      
      tg?.HapticFeedback.impactOccurred('light');
    }

    function openInTelegramPlayer(url) {
      try {
        currentTrackId = 'stream_' + Date.now();
        
        tg.openAudio(url, {
          title: 'Аудиопоток',
          performer: 'Радиостанция',
          audio_id: currentTrackId
        });
        
        elements.telegramNotice.classList.add('show');
        updatePlayerStatus('Открыто в Telegram Player', 'playing');
        updatePlayButton(true);
        
      } catch (error) {
        console.error('Telegram Player error:', error);
        openInNativePlayer(url);
      }
    }

    function openInNativePlayer(url) {
      if (!player) return;
      
      player.src = url;
      player.load();
      elements.telegramNotice.classList.remove('show');
      updatePlayerStatus('Поток загружен. Нажмите "Воспроизвести"');
      updatePlayButton(false);
    }

    async function togglePlayPause() {
      if (isTelegramPlayer) {
        // Для Telegram Player управление через системные кнопки
        tg?.HapticFeedback.impactOccurred('light');
        return;
      }
      
      if (!player || !player.src) {
        loadStream();
        return;
      }
      
      try {
        if (player.paused) {
          player.setAttribute('playsinline', '');
          await player.play();
        } else {
          player.pause();
        }
        tg?.HapticFeedback.impactOccurred('light');
      } catch (error) {
        updatePlayerStatus('Ошибка воспроизведения', 'error');
        tg?.HapticFeedback.notificationOccurred('error');
      }
    }

    function stopPlayback() {
      if (isTelegramPlayer && tg?.closeAudio) {
        tg.closeAudio();
        isTelegramPlayer = false;
        elements.telegramNotice.classList.remove('show');
      }
      
      if (player) {
        player.pause();
        player.currentTime = 0;
      }
      
      updatePlayerStatus('Воспроизведение остановлено');
      updatePlayButton(false);
      tg?.HapticFeedback.impactOccurred('medium');
    }

    function toggleTelegramPlayer() {
      const url = elements.urlInput.value.trim();
      
      if (!isValidUrl(url)) {
        updatePlayerStatus('Сначала введите URL', 'error');
        tg?.HapticFeedback.notificationOccurred('error');
        return;
      }
      
      isTelegramPlayer = !isTelegramPlayer;
      
      if (isTelegramPlayer) {
        elements.telegramBtn.innerHTML = '📱 Нативный плеер';
        openInTelegramPlayer(url);
      } else {
        elements.telegramBtn.innerHTML = '🎵 Telegram Player';
        openInNativePlayer(url);
      }
      
      tg?.HapticFeedback.selectionChanged();
    }

    function updatePlayerStatus(text, type = '') {
      elements.playerStatus.textContent = text;
      elements.playerStatus.className = 'player-status';
      
      if (type) {
        elements.playerStatus.classList.add(type);
      }
    }

    function updatePlayButton(isPlaying) {
      if (isTelegramPlayer) {
        elements.playPauseBtn.innerHTML = '<span>📱</span><span>Telegram Player</span>';
        elements.playPauseBtn.disabled = true;
      } else {
        elements.playPauseBtn.innerHTML = isPlaying 
          ? '<span>⏸</span><span>Пауза</span>'
          : '<span>▶</span><span>Воспроизвести</span>';
        elements.playPauseBtn.disabled = false;
      }
    }

    // Инициализация при загрузке
    document.addEventListener('DOMContentLoaded', init);
    
    // Обработка закрытия приложения
    window.addEventListener('beforeunload', () => {
      if (isTelegramPlayer && tg?.closeAudio) {
        tg.closeAudio();
      }
    });
  </script>
</body>
</html>
