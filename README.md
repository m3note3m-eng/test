<html lang="ru">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1,maximum-scale=1,user-scalable=no" />
  <title>Telegram Audio Stream</title>
  <script src="https://telegram.org/js/telegram-web-app.js"></script>
  <style>
    * {
      margin: 0;
      padding: 0;
      box-sizing: border-box;
    }
    
    :root {
      --bg-primary: rgba(18, 18, 18, 0.95);
      --bg-secondary: rgba(255, 255, 255, 0.1);
      --bg-glass: rgba(255, 255, 255, 0.12);
      --text-primary: #ffffff;
      --text-secondary: rgba(255, 255, 255, 0.7);
      --accent: #3390ec;
      --accent-hover: #2b7fd8;
      --border-glass: rgba(255, 255, 255, 0.18);
      --shadow-glass: 0 8px 32px rgba(0, 0, 0, 0.3);
      --blur: 25px;
      
      /* Размеры для масштабирования */
      --base-size: 16px;
      --scale-factor: 1;
    }
    
    /* Адаптивное масштабирование */
    @media (max-width: 320px) {
      :root {
        --scale-factor: 0.85;
        --base-size: 14px;
      }
    }
    
    @media (min-width: 321px) and (max-width: 375px) {
      :root {
        --scale-factor: 0.9;
        --base-size: 15px;
      }
    }
    
    @media (min-width: 376px) and (max-width: 428px) {
      :root {
        --scale-factor: 0.95;
      }
    }
    
    @media (min-width: 429px) and (max-width: 768px) {
      :root {
        --scale-factor: 1;
        --base-size: 17px;
      }
    }
    
    @media (min-width: 769px) {
      :root {
        --scale-factor: 1.1;
        --base-size: 18px;
      }
    }
    
    html {
      font-size: var(--base-size);
    }
    
    body {
      font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, 'Helvetica Neue', Arial, sans-serif;
      background: linear-gradient(135deg, #0f0f23 0%, #1a1a2e 50%, #16213e 100%);
      color: var(--text-primary);
      min-height: 100vh;
      min-height: 100dvh;
      padding: env(safe-area-inset-top) env(safe-area-inset-right) env(safe-area-inset-bottom) env(safe-area-inset-left);
      display: flex;
      flex-direction: column;
      font-size: calc(1rem * var(--scale-factor));
    }
    
    .container {
      max-width: 100%;
      width: min(100%, 480px);
      margin: 0 auto;
      padding: calc(2rem * var(--scale-factor)) 
               calc(1rem * var(--scale-factor)) 
               calc(4rem * var(--scale-factor));
      display: flex;
      flex-direction: column;
      gap: calc(1.5rem * var(--scale-factor));
      flex: 1;
    }
    
    /* Стиль жидкого стекла */
    .glass {
      background: var(--bg-glass);
      backdrop-filter: blur(var(--blur));
      -webkit-backdrop-filter: blur(var(--blur));
      border: 1px solid var(--border-glass);
      border-radius: calc(1.5rem * var(--scale-factor));
      box-shadow: var(--shadow-glass);
    }
    
    .header {
      padding: calc(1.5rem * var(--scale-factor)) calc(1rem * var(--scale-factor));
      text-align: center;
      margin-top: calc(env(safe-area-inset-top) + 0.5rem);
    }
    
    .title {
      font-size: calc(1.5rem * var(--scale-factor));
      font-weight: 700;
      margin-bottom: calc(0.5rem * var(--scale-factor));
      background: linear-gradient(90deg, #3390ec, #4a9eff);
      -webkit-background-clip: text;
      -webkit-text-fill-color: transparent;
      background-clip: text;
    }
    
    .subtitle {
      font-size: calc(0.875rem * var(--scale-factor));
      color: var(--text-secondary);
      line-height: 1.4;
    }
    
    .input-section {
      padding: calc(1.5rem * var(--scale-factor));
      display: flex;
      flex-direction: column;
      gap: calc(1rem * var(--scale-factor));
    }
    
    .url-input {
      width: 100%;
      padding: calc(1rem * var(--scale-factor)) calc(1.25rem * var(--scale-factor));
      background: rgba(0, 0, 0, 0.25);
      border: 1px solid rgba(255, 255, 255, 0.15);
      border-radius: calc(1rem * var(--scale-factor));
      color: var(--text-primary);
      font-size: calc(1rem * var(--scale-factor));
      outline: none;
      transition: all 0.3s ease;
    }
    
    .url-input:focus {
      border-color: var(--accent);
      background: rgba(0, 0, 0, 0.35);
    }
    
    .url-input::placeholder {
      color: var(--text-secondary);
    }
    
    .presets {
      display: grid;
      grid-template-columns: repeat(auto-fit, minmax(140px, 1fr));
      gap: calc(0.75rem * var(--scale-factor));
      margin-top: calc(0.5rem * var(--scale-factor));
    }
    
    .preset-btn {
      padding: calc(0.75rem * var(--scale-factor)) calc(1rem * var(--scale-factor));
      background: rgba(51, 144, 236, 0.15);
      border: 1px solid rgba(51, 144, 236, 0.25);
      border-radius: calc(0.75rem * var(--scale-factor));
      color: var(--accent);
      font-size: calc(0.875rem * var(--scale-factor));
      cursor: pointer;
      transition: all 0.3s ease;
      display: flex;
      align-items: center;
      justify-content: center;
      gap: calc(0.5rem * var(--scale-factor));
    }
    
    .preset-btn:active {
      transform: scale(0.98);
      background: rgba(51, 144, 236, 0.25);
    }
    
    .player-section {
      padding: calc(1.5rem * var(--scale-factor));
      text-align: center;
    }
    
    .status {
      font-size: calc(1rem * var(--scale-factor));
      margin-bottom: calc(1.5rem * var(--scale-factor));
      min-height: calc(1.5rem * var(--scale-factor));
      font-weight: 500;
      color: var(--text-secondary);
    }
    
    .status.active {
      color: #4cd137;
    }
    
    .status.error {
      color: #ff6b6b;
    }
    
    .player-info {
      background: rgba(51, 144, 236, 0.1);
      border: 1px solid rgba(51, 144, 236, 0.2);
      border-radius: calc(1rem * var(--scale-factor));
      padding: calc(1rem * var(--scale-factor));
      margin-bottom: calc(1.5rem * var(--scale-factor));
      font-size: calc(0.875rem * var(--scale-factor));
      color: var(--accent);
      display: flex;
      align-items: center;
      justify-content: center;
      gap: calc(0.75rem * var(--scale-factor));
    }
    
    .controls {
      display: grid;
      grid-template-columns: 1fr;
      gap: calc(0.75rem * var(--scale-factor));
    }
    
    .control-btn {
      padding: calc(1rem * var(--scale-factor));
      background: var(--accent);
      border: none;
      border-radius: calc(1rem * var(--scale-factor));
      color: white;
      font-size: calc(1rem * var(--scale-factor));
      font-weight: 600;
      cursor: pointer;
      transition: all 0.3s ease;
      display: flex;
      align-items: center;
      justify-content: center;
      gap: calc(0.75rem * var(--scale-factor));
    }
    
    .control-btn:active {
      transform: scale(0.98);
      background: var(--accent-hover);
    }
    
    .control-btn.secondary {
      background: rgba(255, 255, 255, 0.1);
      color: var(--text-primary);
      border: 1px solid rgba(255, 255, 255, 0.15);
    }
    
    .control-btn.secondary:active {
      background: rgba(255, 255, 255, 0.2);
    }
    
    .footer {
      position: fixed;
      bottom: 0;
      left: 0;
      right: 0;
      padding: calc(1rem * var(--scale-factor)) 
               calc(1rem * var(--scale-factor)) 
               calc(env(safe-area-inset-bottom) + 1rem * var(--scale-factor));
      background: linear-gradient(transparent, rgba(0, 0, 0, 0.85));
      backdrop-filter: blur(10px);
      -webkit-backdrop-filter: blur(10px);
      z-index: 100;
    }
    
    .action-bar {
      display: grid;
      grid-template-columns: 1fr;
      gap: calc(0.75rem * var(--scale-factor));
    }
    
    .action-btn {
      padding: calc(1rem * var(--scale-factor));
      background: var(--accent);
      border: none;
      border-radius: calc(1rem * var(--scale-factor));
      color: white;
      font-size: calc(1rem * var(--scale-factor));
      font-weight: 600;
      cursor: pointer;
      transition: all 0.3s ease;
      display: flex;
      align-items: center;
      justify-content: center;
      gap: calc(0.75rem * var(--scale-factor));
    }
    
    .action-btn:active {
      transform: scale(0.98);
    }
    
    /* Анимации */
    @keyframes fadeIn {
      from {
        opacity: 0;
        transform: translateY(10px);
      }
      to {
        opacity: 1;
        transform: translateY(0);
      }
    }
    
    .container > * {
      animation: fadeIn 0.4s ease-out;
    }
    
    @keyframes pulse {
      0% { opacity: 1; }
      50% { opacity: 0.7; }
      100% { opacity: 1; }
    }
    
    .loading {
      animation: pulse 1.5s infinite;
    }
    
    /* Адаптация для ландшафта */
    @media (orientation: landscape) and (max-height: 500px) {
      .container {
        flex-direction: row;
        flex-wrap: wrap;
        padding-bottom: calc(3.5rem * var(--scale-factor));
      }
      
      .header {
        flex: 0 0 100%;
        margin-top: 0;
        padding: calc(1rem * var(--scale-factor));
      }
      
      .input-section {
        flex: 1;
        min-width: 250px;
      }
      
      .player-section {
        flex: 1;
        min-width: 250px;
      }
      
      .presets {
        grid-template-columns: 1fr;
      }
    }
    
    /* Темная тема Telegram */
    @media (prefers-color-scheme: light) {
      :root {
        --bg-primary: rgba(255, 255, 255, 0.95);
        --bg-secondary: rgba(0, 0, 0, 0.1);
        --bg-glass: rgba(0, 0, 0, 0.08);
        --text-primary: #000000;
        --text-secondary: rgba(0, 0, 0, 0.7);
        --border-glass: rgba(0, 0, 0, 0.12);
        --shadow-glass: 0 8px 32px rgba(0, 0, 0, 0.1);
      }
      
      body {
        background: linear-gradient(135deg, #f5f5f5 0%, #e8e8e8 50%, #e0e0e0 100%);
      }
      
      .url-input {
        background: rgba(255, 255, 255, 0.7);
        border-color: rgba(0, 0, 0, 0.1);
        color: #000000;
      }
      
      .player-info {
        background: rgba(51, 144, 236, 0.08);
        border-color: rgba(51, 144, 236, 0.15);
      }
      
      .control-btn.secondary {
        background: rgba(0, 0, 0, 0.08);
        border-color: rgba(0, 0, 0, 0.12);
        color: #000000;
      }
    }
  </style>
</head>
<body>
  <div class="container">
    <header class="header glass">
      <h1 class="title">Telegram Audio Stream</h1>
      <p class="subtitle">Только Telegram Player. Фоновое воспроизведение с уведомлениями.</p>
    </header>
    
    <section class="input-section glass">
      <input 
        type="url" 
        class="url-input" 
        id="urlInput"
        placeholder="https://media.example.com/stream.mp3"
        inputmode="url"
        autocomplete="off"
      />
      
      <div class="presets">
        <button class="preset-btn" data-url="https://media2.datacenter.by/stream/stalica/stream">
          🎵 Stalica
        </button>
        <button class="preset-btn" data-url="https://stream.radiopub.ru/radiorecord_lofi">
          ☕ LoFi
        </button>
        <button class="preset-btn" data-url="https://icecast.radiofrance.fr/fip-midfi.mp3">
          🇫🇷 FIP
        </button>
      </div>
    </section>
    
    <section class="player-section glass">
      <div class="status" id="status">Введите URL и нажмите "Открыть в плеере"</div>
      
      <div class="player-info" id="playerInfo">
        <span>🎧</span>
        <span>Используется Telegram Player</span>
      </div>
      
      <div class="controls">
        <button class="control-btn" id="openPlayerBtn">
          <span>▶️</span>
          <span>Открыть в плеере</span>
        </button>
        <button class="control-btn secondary" id="stopBtn">
          <span>⏹️</span>
          <span>Остановить</span>
        </button>
      </div>
    </section>
  </div>
  
  <footer class="footer">
    <div class="action-bar">
      <button class="action-btn" id="playBtn">
        <span>🎵</span>
        <span>Воспроизвести в Telegram</span>
      </button>
    </div>
  </footer>

  <script>
    // Telegram WebApp
    const tg = window.Telegram?.WebApp;
    let currentTrackId = null;
    let isPlaying = false;

    // Элементы
    const elements = {
      urlInput: document.getElementById('urlInput'),
      status: document.getElementById('status'),
      playerInfo: document.getElementById('playerInfo'),
      openPlayerBtn: document.getElementById('openPlayerBtn'),
      stopBtn: document.getElementById('stopBtn'),
      playBtn: document.getElementById('playBtn'),
      presetBtns: document.querySelectorAll('.preset-btn')
    };

    // Инициализация
    function init() {
      if (tg) {
        tg.ready();
        tg.expand();
        applyTelegramTheme();
        tg.onEvent('themeChanged', applyTelegramTheme);
      }

      // Восстановить последний URL
      const lastUrl = localStorage.getItem('tg_stream_url');
      if (lastUrl) {
        elements.urlInput.value = lastUrl;
        updateStatus('Готов к воспроизведению');
      }

      // Обработчики событий
      setupEventListeners();
      
      // Проверка доступности Telegram Player
      if (!tg?.openAudio) {
        updateStatus('Telegram Player недоступен', 'error');
        elements.openPlayerBtn.disabled = true;
        elements.playBtn.disabled = true;
      }
    }

    function applyTelegramTheme() {
      if (!tg || !tg.themeParams) return;
      
      const theme = tg.themeParams;
      document.documentElement.style.setProperty('--accent', theme.link_color || '#3390ec');
      
      // Адаптация под светлую тему
      if (tg.colorScheme === 'light') {
        document.documentElement.style.setProperty('--bg-glass', 'rgba(0, 0, 0, 0.08)');
        document.documentElement.style.setProperty('--text-primary', '#000000');
        document.documentElement.style.setProperty('--text-secondary', 'rgba(0, 0, 0, 0.7)');
      }
    }

    function setupEventListeners() {
      // Открыть в плеере
      elements.openPlayerBtn.addEventListener('click', openInTelegramPlayer);
      
      // Воспроизвести
      elements.playBtn.addEventListener('click', openInTelegramPlayer);
      
      // Остановить
      elements.stopBtn.addEventListener('click', stopPlayback);
      
      // Пресеты
      elements.presetBtns.forEach(btn => {
        btn.addEventListener('click', () => {
          const url = btn.dataset.url;
          elements.urlInput.value = url;
          updateStatus(`Выбрана станция: ${btn.textContent.trim()}`);
          tg?.HapticFeedback.selectionChanged();
        });
      });
      
      // Enter для запуска
      elements.urlInput.addEventListener('keypress', (e) => {
        if (e.key === 'Enter') openInTelegramPlayer();
      });
      
      // Обработка видимости
      document.addEventListener('visibilitychange', () => {
        if (!document.hidden && isPlaying) {
          updateStatus('Идёт воспроизведение', 'active');
        }
      });
    }

    function isValidUrl(url) {
      try {
        const urlObj = new URL(url);
        return ['http:', 'https:'].includes(urlObj.protocol);
      } catch {
        return false;
      }
    }

    function openInTelegramPlayer() {
      const url = elements.urlInput.value.trim();
      
      if (!isValidUrl(url)) {
        updateStatus('Введите корректный URL', 'error');
        tg?.HapticFeedback.notificationOccurred('error');
        return;
      }
      
      // Сохранить URL
      localStorage.setItem('tg_stream_url', url);
      
      if (!tg?.openAudio) {
        updateStatus('Telegram Player недоступен', 'error');
        return;
      }
      
      try {
        currentTrackId = 'tg_stream_' + Date.now();
        
        // Открываем аудио в Telegram Player
        tg.openAudio(url, {
          title: getStreamName(url),
          performer: 'Аудиопоток',
          audio_id: currentTrackId
        });
        
        isPlaying = true;
        updateStatus('Открыто в Telegram Player', 'active');
        elements.playerInfo.style.display = 'flex';
        
        tg?.HapticFeedback.impactOccurred('medium');
        
      } catch (error) {
        console.error('Ошибка Telegram Player:', error);
        updateStatus('Ошибка открытия потока', 'error');
        tg?.HapticFeedback.notificationOccurred('error');
      }
    }

    function getStreamName(url) {
      try {
        const hostname = new URL(url).hostname;
        const presetNames = {
          'datacenter.by': 'Stalica Radio',
          'radiopub.ru': 'LoFi Radio',
          'radiofrance.fr': 'FIP Radio'
        };
        
        for (const [domain, name] of Object.entries(presetNames)) {
          if (hostname.includes(domain)) return name;
        }
        
        return 'Аудиопоток';
      } catch {
        return 'Аудиопоток';
      }
    }

    function stopPlayback() {
      if (tg?.closeAudio) {
        try {
          tg.closeAudio();
          isPlaying = false;
          updateStatus('Воспроизведение остановлено');
          elements.playerInfo.style.display = 'none';
          tg?.HapticFeedback.impactOccurred('light');
        } catch (error) {
          console.error('Ошибка остановки:', error);
        }
      }
    }

    function updateStatus(text, type = '') {
      elements.status.textContent = text;
      elements.status.className = 'status';
      
      if (type) {
        elements.status.classList.add(type);
      }
    }

    // Инициализация при загрузке
    document.addEventListener('DOMContentLoaded', init);
    
    // Остановка при закрытии
    window.addEventListener('beforeunload', () => {
      if (isPlaying && tg?.closeAudio) {
        tg.closeAudio();
      }
    });
  </script>
</body>
</html>
