<html lang="ru">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Telegram Radio Stream</title>
  <style>
    :root {
      --primary-color: #0088cc;
      --bg-color: var(--tg-theme-bg-color, #f0f0f0);
      --text-color: var(--tg-theme-text-color, #000000);
      --secondary-bg: var(--tg-theme-secondary-bg-color, #ffffff);
    }
    
    body {
      font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;
      background-color: var(--bg-color);
      color: var(--text-color);
      margin: 0;
      padding: 16px;
      transition: background-color 0.3s;
    }
    
    .container {
      background: var(--secondary-bg);
      border-radius: 12px;
      padding: 20px;
      box-shadow: 0 4px 20px rgba(0,0,0,0.1);
      max-width: 500px;
      margin: 0 auto;
    }
    
    h3 {
      margin-top: 0;
      margin-bottom: 20px;
      display: flex;
      align-items: center;
      gap: 10px;
    }
    
    .input-group {
      margin-bottom: 15px;
    }
    
    input[type="text"] {
      width: 100%;
      padding: 12px;
      border-radius: 8px;
      border: 2px solid #e0e0e0;
      background: var(--tg-theme-bg-color, #ffffff);
      color: var(--text-color);
      font-size: 16px;
      box-sizing: border-box;
      transition: border-color 0.3s;
    }
    
    input[type="text"]:focus {
      outline: none;
      border-color: var(--primary-color);
    }
    
    .button-group {
      display: flex;
      gap: 10px;
      margin-bottom: 20px;
    }
    
    button {
      flex: 1;
      background-color: var(--primary-color);
      color: white;
      border: none;
      padding: 14px;
      border-radius: 8px;
      cursor: pointer;
      font-size: 16px;
      font-weight: 600;
      transition: all 0.2s;
      display: flex;
      align-items: center;
      justify-content: center;
      gap: 8px;
    }
    
    button:hover {
      opacity: 0.9;
      transform: translateY(-1px);
    }
    
    button:active {
      transform: translateY(0);
    }
    
    button:disabled {
      opacity: 0.5;
      cursor: not-allowed;
      transform: none;
    }
    
    #startBtn {
      background-color: #2ecc71;
    }
    
    #stopBtn {
      background-color: #e74c3c;
    }
    
    #saveBtn {
      background-color: #3498db;
    }
    
    audio {
      width: 100%;
      margin-top: 20px;
      border-radius: 8px;
    }
    
    .status {
      padding: 10px;
      border-radius: 8px;
      margin-top: 15px;
      text-align: center;
      font-weight: 500;
      display: none;
    }
    
    .status.playing {
      background-color: rgba(46, 204, 113, 0.2);
      color: #27ae60;
      display: block;
    }
    
    .status.stopped {
      background-color: rgba(231, 76, 60, 0.2);
      color: #c0392b;
      display: block;
    }
    
    .error {
      background-color: rgba(231, 76, 60, 0.2);
      color: #c0392b;
      padding: 10px;
      border-radius: 8px;
      margin-top: 10px;
      display: none;
    }
    
    .stations {
      margin-top: 20px;
    }
    
    .station-item {
      padding: 10px;
      margin: 5px 0;
      background: rgba(0, 136, 204, 0.1);
      border-radius: 6px;
      cursor: pointer;
      transition: background 0.2s;
    }
    
    .station-item:hover {
      background: rgba(0, 136, 204, 0.2);
    }
    
    .volume-control {
      margin: 15px 0;
      display: flex;
      align-items: center;
      gap: 10px;
    }
    
    .volume-control input {
      flex: 1;
    }
  </style>
</head>
<body>
  <div class="container">
    <h3>📻 Telegram Radio</h3>
    
    <div class="input-group">
      <input type="text" id="streamUrl" 
             placeholder="https://radio.example.com:8000/stream"
             autocomplete="off">
    </div>
    
    <div class="button-group">
      <button id="startBtn" onclick="startStream()">
        ▶ Старт
      </button>
      <button id="stopBtn" onclick="stopStream()" disabled>
        ⏹ Стоп
      </button>
      <button id="saveBtn" onclick="saveStation()">
        💾 Сохранить
      </button>
    </div>
    
    <div class="volume-control">
      <span>🔊</span>
      <input type="range" id="volume" min="0" max="1" step="0.1" value="1">
      <span id="volumeValue">100%</span>
    </div>
    
    <div id="status" class="status"></div>
    <div id="error" class="error"></div>
    
    <audio id="radioPlayer" controls playsinline></audio>
    
    <div class="stations" id="stationsList" style="display: none;">
      <h4>Сохранённые станции:</h4>
      <div id="stations"></div>
    </div>
  </div>

  <script src="https://telegram.org/js/telegram-web-app.js"></script>
  <script>
    const player = document.getElementById('radioPlayer');
    const startBtn = document.getElementById('startBtn');
    const stopBtn = document.getElementById('stopBtn');
    const statusDiv = document.getElementById('status');
    const errorDiv = document.getElementById('error');
    const volumeSlider = document.getElementById('volume');
    const volumeValue = document.getElementById('volumeValue');
    const stationsList = document.getElementById('stations');
    const stationsContainer = document.getElementById('stationsList');
    
    let isPlaying = false;
    let savedStations = [];
    
    // Инициализация Telegram WebApp
    Telegram.WebApp.ready();
    Telegram.WebApp.expand();
    
    // Настройка темы Telegram
    function updateTheme() {
      document.body.style.backgroundColor = Telegram.WebApp.backgroundColor;
      document.body.style.color = Telegram.WebApp.textColor;
      
      // Обновляем CSS переменные
      document.documentElement.style.setProperty('--tg-theme-bg-color', Telegram.WebApp.backgroundColor);
      document.documentElement.style.setProperty('--tg-theme-text-color', Telegram.WebApp.textColor);
      document.documentElement.style.setProperty('--tg-theme-secondary-bg-color', Telegram.WebApp.secondaryBackgroundColor);
    }
    
    // Загрузка сохранённых станций
    function loadStations() {
      const saved = localStorage.getItem('radioStations');
      if (saved) {
        savedStations = JSON.parse(saved);
        displayStations();
      }
    }
    
    // Отображение сохранённых станций
    function displayStations() {
      if (savedStations.length > 0) {
        stationsContainer.style.display = 'block';
        stationsList.innerHTML = savedStations.map((station, index) => `
          <div class="station-item" onclick="loadStation(${index})">
            ${station.name || `Станция ${index + 1}`}
          </div>
        `).join('');
      }
    }
    
    // Загрузка станции
    function loadStation(index) {
      if (savedStations[index]) {
        document.getElementById('streamUrl').value = savedStations[index].url;
        startStream();
      }
    }
    
    // Сохранение станции
    function saveStation() {
      const url = document.getElementById('streamUrl').value.trim();
      if (!url) {
        showError('Введите URL станции');
        return;
      }
      
      const stationName = prompt('Название станции:', `Станция ${savedStations.length + 1}`);
      if (stationName) {
        savedStations.push({
          name: stationName,
          url: url
        });
        
        localStorage.setItem('radioStations', JSON.stringify(savedStations));
        displayStations();
        showStatus(`Станция "${stationName}" сохранена`, 'success');
      }
    }
    
    // Управление громкостью
    volumeSlider.addEventListener('input', (e) => {
      const volume = e.target.value;
      player.volume = volume;
      volumeValue.textContent = `${volume * 100}%`;
    });
    
    // Запуск стрима
    function startStream() {
      const url = document.getElementById('streamUrl').value.trim();
      
      if (!url) {
        showError('Введите ссылку на радио‑стрим!');
        return;
      }
      
      // Проверка URL
      if (!url.startsWith('http')) {
        showError('URL должен начинаться с http:// или https://');
        return;
      }
      
      showStatus('Подключаемся...', 'loading');
      hideError();
      
      player.src = url;
      player.volume = volumeSlider.value;
      
      player.play().then(() => {
        isPlaying = true;
        updateButtons();
        showStatus('Стрим запущен', 'playing');
        
        // Отправляем данные в Telegram
        if (Telegram.WebApp.sendData) {
          Telegram.WebApp.sendData(JSON.stringify({
            action: 'stream_started',
            url: url,
            timestamp: new Date().toISOString()
          }));
        }
      }).catch(err => {
        showError('Ошибка воспроизведения: ' + err.message);
        isPlaying = false;
        updateButtons();
      });
    }
    
    // Остановка стрима
    function stopStream() {
      player.pause();
      player.src = '';
      isPlaying = false;
      updateButtons();
      showStatus('Стрим остановлен', 'stopped');
      
      if (Telegram.WebApp.sendData) {
        Telegram.WebApp.sendData(JSON.stringify({
          action: 'stream_stopped',
          timestamp: new Date().toISOString()
        }));
      }
    }
    
    // Обновление состояния кнопок
    function updateButtons() {
      startBtn.disabled = isPlaying;
      stopBtn.disabled = !isPlaying;
    }
    
    // Показать статус
    function showStatus(message, type) {
      statusDiv.textContent = message;
      statusDiv.className = 'status ' + type;
    }
    
    // Показать ошибку
    function showError(message) {
      errorDiv.textContent = message;
      errorDiv.style.display = 'block';
    }
    
    // Скрыть ошибку
    function hideError() {
      errorDiv.style.display = 'none';
    }
    
    // Обработчики событий плеера
    player.addEventListener('error', (e) => {
      showError('Ошибка загрузки стрима. Проверьте URL.');
      isPlaying = false;
      updateButtons();
    });
    
    player.addEventListener('pause', () => {
      if (isPlaying) {
        isPlaying = false;
        updateButtons();
        showStatus('Стрим на паузе', 'stopped');
      }
    });
    
    player.addEventListener('playing', () => {
      isPlaying = true;
      updateButtons();
      showStatus('Стрим запущен', 'playing');
    });
    
    // Инициализация
    updateTheme();
    loadStations();
    
    // Обработка смены темы
    Telegram.WebApp.onEvent('themeChanged', updateTheme);
    
    // Автостарт последней станции (опционально)
    window.addEventListener('load', () => {
      const lastStation = localStorage.getItem('lastStation');
      if (lastStation) {
        document.getElementById('streamUrl').value = lastStation;
      }
    });
    
    // Сохранение последней станции
    player.addEventListener('play', () => {
      localStorage.setItem('lastStation', player.src);
    });
  </script>
</body>
</html>
