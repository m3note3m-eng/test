<html lang="ru">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>Audio Stream WebApp</title>
  <script src="https://telegram.org/js/telegram-web-app.js"></script>
  <style>
    :root {
      --bg: #111;
      --text: #fff;
      --hint: #9aa0a6;
      --accent: #2ea6ff;
      --card: #1a1a1a;
      --border: #2a2a2a;
    }
    html, body {
      height: 100%;
      margin: 0;
      background: var(--bg);
      color: var(--text);
      font: 16px/1.4 system-ui, -apple-system, Segoe UI, Roboto, Ubuntu, "Helvetica Neue", Arial, "Noto Sans", sans-serif;
    }
    .wrap {
      max-width: 720px;
      margin: 0 auto;
      padding: 16px;
    }
    header {
      margin-bottom: 12px;
    }
    h1 {
      font-size: 18px;
      margin: 0 0 4px;
    }
    .hint {
      color: var(--hint);
      font-size: 13px;
    }
    .card {
      background: var(--card);
      border: 1px solid var(--border);
      border-radius: 12px;
      padding: 14px;
      display: grid;
      gap: 12px;
    }
    .row {
      display: grid;
      grid-template-columns: 1fr auto;
      gap: 8px;
      align-items: center;
    }
    input[type="url"] {
      width: 100%;
      padding: 10px 12px;
      border-radius: 10px;
      border: 1px solid var(--border);
      background: transparent;
      color: var(--text);
      outline: none;
      font-size: 15px;
    }
    input::placeholder { color: var(--hint); }
    button {
      padding: 10px 14px;
      border-radius: 10px;
      border: 1px solid var(--border);
      background: var(--accent);
      color: #fff;
      font-weight: 600;
      cursor: pointer;
    }
    button.secondary {
      background: transparent;
      color: var(--text);
    }
    .status {
      font-size: 13px;
      color: var(--hint);
    }
    audio {
      width: 100%;
      height: 36px;
    }
    .grid {
      display: grid;
      grid-template-columns: 1fr 1fr;
      gap: 8px;
    }
    .error { color: #ff6b6b; }
    .ok { color: #4cd137; }
    footer {
      margin-top: 16px;
      font-size: 12px;
      color: var(--hint);
    }
  </style>
</head>
<body>
  <div class="wrap">
    <header>
      <h1>Потоковое аудио</h1>
      <div class="hint">Вставьте ссылку на поток (например, MP3/AAC/OGG). Пример: https://media2.datacenter.by/stream/stalica/stream</div>
    </header>

    <section class="card">
      <div class="row">
        <input id="url" type="url" placeholder="https://..." inputmode="url" autocomplete="off" />
        <button id="load">Загрузить</button>
      </div>

      <audio id="player" controls preload="none"></audio>

      <div class="grid">
        <button id="playPause">▶︎ Воспроизвести</button>
        <button id="stop" class="secondary">■ Стоп</button>
      </div>

      <div id="status" class="status">Готово.</div>
    </section>

    <footer>
      Если поток не стартует: проверьте CORS/HTTPS, формат кодека и политику автозапуска браузера.
    </footer>
  </div>

  <script>
    const tg = window.Telegram?.WebApp;
    if (tg) {
      tg.ready();
      tg.expand();
      applyTheme(tg.themeParams);
      tg.onEvent('themeChanged', () => applyTheme(tg.themeParams));
    }

    function applyTheme(tp = {}) {
      const css = document.documentElement.style;
      if (tp.bg_color) css.setProperty('--bg', tp.bg_color);
      if (tp.text_color) css.setProperty('--text', tp.text_color);
      if (tp.hint_color) css.setProperty('--hint', tp.hint_color);
      if (tp.link_color) css.setProperty('--accent', tp.link_color);
      if (tp.secondary_bg_color) css.setProperty('--card', tp.secondary_bg_color);
      if (tp.section_separator_color) css.setProperty('--border', tp.section_separator_color);
    }

    const urlInput = document.getElementById('url');
    const loadBtn = document.getElementById('load');
    const playPauseBtn = document.getElementById('playPause');
    const stopBtn = document.getElementById('stop');
    const statusEl = document.getElementById('status');
    const player = document.getElementById('player');

    // Инициализация Media Session API для фонового воспроизведения
    let mediaSessionInitialized = false;

    const last = localStorage.getItem('stream_url');
    if (last) urlInput.value = last;

    player.addEventListener('playing', () => {
      setStatus('Идёт воспроизведение.', 'ok');
      initMediaSession();
    });
    player.addEventListener('pause', () => setStatus('Пауза.'));
    player.addEventListener('waiting', () => setStatus('Буферизация…'));
    player.addEventListener('ended', () => {
      setStatus('Поток завершён.');
      updatePlayButton(false);
    });
    player.addEventListener('error', () => {
      const code = player.error?.code;
      setStatus('Ошибка воспроизведения' + (code ? ` (код ${code})` : '') + '.', 'error');
      updatePlayButton(false);
    });

    loadBtn.addEventListener('click', loadStream);
    playPauseBtn.addEventListener('click', playPause);
    stopBtn.addEventListener('click', stop);

    // Обработчик видимости страницы для оптимизации фонового воспроизведения
    document.addEventListener('visibilitychange', () => {
      if (document.hidden && !player.paused) {
        // Страница скрыта, но воспроизведение продолжается
        console.log('Приложение ушло в фон, воспроизведение продолжается');
      }
    });

    function setStatus(text, kind) {
      statusEl.textContent = text;
      statusEl.className = 'status' + (kind ? ' ' + kind : '');
    }

    function isValidUrl(u) {
      try {
        const x = new URL(u);
        return ['http:', 'https:'].includes(x.protocol);
      } catch { return false; }
    }

    function loadStream() {
      const u = urlInput.value.trim();
      if (!isValidUrl(u)) {
        setStatus('Некорректный URL. Укажите ссылку вида https://…', 'error');
        return;
      }
      localStorage.setItem('stream_url', u);
      player.src = u;
      player.load();
      setStatus('Поток загружен. Нажмите воспроизведение.');
      updatePlayButton(false);
    }

    async function playPause() {
      if (!player.src) {
        loadStream();
        if (!player.src) return;
      }
      if (player.paused) {
        try {
          // Устанавливаем флаг для фонового воспроизведения
          player.setAttribute('playsinline', '');
          player.setAttribute('webkit-playsinline', '');
          
          await player.play();
          updatePlayButton(true);
          
          // Запрашиваем разрешение на фоновое воспроизведение (если нужно)
          if ('wakeLock' in navigator) {
            try {
              const wakeLock = await navigator.wakeLock.request('screen');
              wakeLock.addEventListener('release', () => {
                console.log('Wake Lock был освобожден');
              });
            } catch (err) {
              console.log('Wake Lock не поддерживается или ошибка:', err);
            }
          }
        } catch (err) {
          setStatus('Автовоспроизведение заблокировано. Нажмите кнопку Play.', 'error');
        }
      } else {
        player.pause();
        updatePlayButton(false);
        
        // Освобождаем Wake Lock при паузе
        if ('wakeLock' in navigator && window.wakeLock) {
          window.wakeLock.release().then(() => {
            window.wakeLock = null;
          });
        }
      }
    }

    function initMediaSession() {
      if (mediaSessionInitialized || !('mediaSession' in navigator)) return;
      
      try {
        // Устанавливаем метаданные для медиасессии
        navigator.mediaSession.metadata = new MediaMetadata({
          title: 'Аудиопоток',
          artist: urlInput.value || 'Пользовательский поток',
          album: 'Telegram WebApp Audio'
        });

        // Устанавливаем обработчики действий
        navigator.mediaSession.setActionHandler('play', () => {
          if (player.paused) playPause();
        });

        navigator.mediaSession.setActionHandler('pause', () => {
          if (!player.paused) playPause();
        });

        navigator.mediaSession.setActionHandler('stop', stop);

        // Обновляем состояние воспроизведения
        navigator.mediaSession.playbackState = player.paused ? 'paused' : 'playing';

        mediaSessionInitialized = true;
      } catch (error) {
        console.log('Media Session API не поддерживается:', error);
      }
    }

    function stop() {
      if (!player.src) return;
      player.pause();
      player.removeAttribute('src');
      player.load();
      updatePlayButton(false);
      setStatus('Остановлено.');
      
      // Очищаем медиасессию
      if ('mediaSession' in navigator) {
        navigator.mediaSession.metadata = null;
        navigator.mediaSession.playbackState = 'none';
      }
      
      mediaSessionInitialized = false;
      
      // Освобождаем Wake Lock
      if ('wakeLock' in navigator && window.wakeLock) {
        window.wakeLock.release().then(() => {
          window.wakeLock = null;
        });
      }
    }

    function updatePlayButton(isPlaying) {
      playPauseBtn.textContent = isPlaying ? '⏸ Пауза' : '▶︎ Воспроизвести';
      
      // Обновляем состояние медиасессии
      if ('mediaSession' in navigator && mediaSessionInitialized) {
        navigator.mediaSession.playbackState = isPlaying ? 'playing' : 'paused';
      }
    }

    urlInput.addEventListener('keydown', (e) => {
      if (e.key === 'Enter') loadStream();
    });
  </script>
</body>
</html>
