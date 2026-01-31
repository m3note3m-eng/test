<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Аудиоплеер | Telegram</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;
        }

        body {
            background-color: var(--tg-theme-bg-color, #18222d);
            color: var(--tg-theme-text-color, #ffffff);
            min-height: 100vh;
            padding: 10px;
        }

        .container {
            max-width: 500px;
            margin: 0 auto;
        }

        header {
            text-align: center;
            margin: 10px 0 20px;
        }

        h1 {
            font-size: 20px;
            color: var(--tg-theme-text-color, #ffffff);
        }

        .player {
            background-color: var(--tg-theme-secondary-bg-color, #232e3c);
            border-radius: 15px;
            padding: 20px;
            margin-bottom: 20px;
        }

        .track-info {
            text-align: center;
            margin-bottom: 20px;
        }

        .track-title {
            font-size: 18px;
            font-weight: 600;
            margin-bottom: 5px;
        }

        .track-artist {
            font-size: 14px;
            color: var(--tg-theme-hint-color, #aaa);
        }

        .progress-container {
            margin: 20px 0;
        }

        .progress-bar {
            width: 100%;
            height: 4px;
            background-color: rgba(255, 255, 255, 0.1);
            border-radius: 2px;
            margin-bottom: 5px;
            cursor: pointer;
            overflow: hidden;
        }

        .progress {
            height: 100%;
            background: linear-gradient(90deg, #6a11cb, #2575fc);
            width: 0%;
        }

        .time-info {
            display: flex;
            justify-content: space-between;
            font-size: 12px;
            color: var(--tg-theme-hint-color, #aaa);
        }

        .controls {
            display: flex;
            justify-content: center;
            align-items: center;
            gap: 20px;
            margin: 20px 0;
        }

        .control-btn {
            background: none;
            border: none;
            color: var(--tg-theme-button-color, #50a8eb);
            font-size: 20px;
            cursor: pointer;
            width: 44px;
            height: 44px;
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
        }

        .play-pause {
            width: 56px;
            height: 56px;
            font-size: 24px;
            background-color: var(--tg-theme-button-color, #50a8eb);
            color: var(--tg-theme-button-text-color, #ffffff);
        }

        .volume-container {
            display: flex;
            align-items: center;
            gap: 10px;
            margin: 15px 0;
        }

        .volume-slider {
            flex-grow: 1;
            height: 4px;
            background-color: rgba(255, 255, 255, 0.1);
            border-radius: 2px;
            cursor: pointer;
        }

        .volume-level {
            height: 100%;
            background-color: var(--tg-theme-button-color, #50a8eb);
            width: 70%;
        }

        .url-input-container {
            margin-top: 20px;
        }

        .url-input {
            width: 100%;
            padding: 12px;
            border-radius: 10px;
            border: none;
            background-color: var(--tg-theme-bg-color, #1a2532);
            color: var(--tg-theme-text-color, #ffffff);
            font-size: 14px;
            margin-bottom: 10px;
        }

        .url-input:focus {
            outline: 2px solid var(--tg-theme-button-color, #50a8eb);
        }

        .url-button {
            background-color: var(--tg-theme-button-color, #50a8eb);
            color: var(--tg-theme-button-text-color, #ffffff);
            border: none;
            padding: 12px;
            border-radius: 10px;
            font-size: 14px;
            font-weight: 600;
            cursor: pointer;
            width: 100%;
        }

        .status {
            text-align: center;
            font-size: 12px;
            color: var(--tg-theme-hint-color, #777);
            margin-top: 10px;
            display: none;
        }

        .status.active {
            display: block;
        }
    </style>
</head>
<body>
    <div class="container">
        <header>
            <h1>Аудиоплеер</h1>
        </header>
        
        <div class="player">
            <div class="track-info">
                <div class="track-title" id="trackTitle">Аудиопоток</div>
                <div class="track-artist" id="trackArtist">Telegram Mini App</div>
            </div>
            
            <div class="progress-container">
                <div class="progress-bar" id="progressBar">
                    <div class="progress" id="progress"></div>
                </div>
                <div class="time-info">
                    <span id="currentTime">0:00</span>
                    <span id="duration">0:00</span>
                </div>
            </div>
            
            <div class="controls">
                <button class="control-btn" id="prevBtn" title="Назад">
                    ⏮
                </button>
                
                <button class="control-btn play-pause" id="playPauseBtn" title="Воспроизвести">
                    ▶
                </button>
                
                <button class="control-btn" id="nextBtn" title="Вперед">
                    ⏭
                </button>
            </div>
            
            <div class="volume-container">
                <span>🔈</span>
                <div class="volume-slider" id="volumeSlider">
                    <div class="volume-level" id="volumeLevel"></div>
                </div>
                <span>🔊</span>
            </div>
            
            <div class="url-input-container">
                <input type="url" class="url-input" id="audioUrl" 
                       placeholder="https://example.com/audio.mp3" 
                       value="https://www.soundhelix.com/examples/mp3/SoundHelix-Song-1.mp3">
                <button class="url-button" id="loadUrlBtn">
                    Загрузить аудио
                </button>
            </div>
        </div>
        
        <div class="status" id="status"></div>
    </div>

    <script>
        // Telegram WebApp API
        const tg = window.Telegram?.WebApp;
        
        // Инициализация Telegram
        if (tg) {
            tg.expand();
            tg.ready();
        }
        
        // Элементы DOM
        const audio = new Audio();
        const playPauseBtn = document.getElementById('playPauseBtn');
        const progressBar = document.getElementById('progressBar');
        const progress = document.getElementById('progress');
        const currentTimeEl = document.getElementById('currentTime');
        const durationEl = document.getElementById('duration');
        const volumeSlider = document.getElementById('volumeSlider');
        const volumeLevel = document.getElementById('volumeLevel');
        const audioUrlInput = document.getElementById('audioUrl');
        const loadUrlBtn = document.getElementById('loadUrlBtn');
        const trackTitle = document.getElementById('trackTitle');
        const trackArtist = document.getElementById('trackArtist');
        const statusEl = document.getElementById('status');
        
        // Состояние
        let isPlaying = false;
        audio.volume = 0.7;
        
        // Форматирование времени
        const formatTime = (seconds) => {
            const mins = Math.floor(seconds / 60);
            const secs = Math.floor(seconds % 60);
            return `${mins}:${secs < 10 ? '0' : ''}${secs}`;
        };
        
        // Обновление прогресса
        const updateProgress = () => {
            if (audio.duration) {
                const percent = (audio.currentTime / audio.duration) * 100;
                progress.style.width = `${percent}%`;
                currentTimeEl.textContent = formatTime(audio.currentTime);
            }
        };
        
        // Установка прогресса
        const setProgress = (e) => {
            const width = progressBar.clientWidth;
            const clickX = e.offsetX;
            if (audio.duration) {
                audio.currentTime = (clickX / width) * audio.duration;
            }
        };
        
        // Установка громкости
        const setVolume = (e) => {
            const width = volumeSlider.clientWidth;
            const clickX = e.offsetX;
            const volume = Math.max(0, Math.min(1, clickX / width));
            audio.volume = volume;
            volumeLevel.style.width = `${volume * 100}%`;
        };
        
        // Переключение воспроизведения
        const togglePlay = () => {
            if (!audio.src) {
                loadAudio();
                return;
            }
            
            if (isPlaying) {
                audio.pause();
                playPauseBtn.innerHTML = '▶';
                showStatus('Пауза');
            } else {
                audio.play().catch(e => {
                    showStatus('Ошибка воспроизведения');
                    console.error(e);
                });
                playPauseBtn.innerHTML = '⏸';
                showStatus('Воспроизведение');
            }
            isPlaying = !isPlaying;
        };
        
        // Загрузка аудио
        const loadAudio = () => {
            const url = audioUrlInput.value.trim();
            if (!url) return;
            
            showStatus('Загрузка...');
            audio.src = url;
            audio.load();
            
            audio.onloadedmetadata = () => {
                durationEl.textContent = formatTime(audio.duration);
                const name = url.split('/').pop().replace(/\.[^/.]+$/, "") || 'Аудио';
                trackTitle.textContent = decodeURIComponent(name);
                togglePlay();
            };
            
            audio.onerror = () => {
                showStatus('Ошибка загрузки');
                audio.src = '';
            };
        };
        
        // Показать статус
        const showStatus = (text) => {
            statusEl.textContent = text;
            statusEl.classList.add('active');
            setTimeout(() => statusEl.classList.remove('active'), 2000);
        };
        
        // Инициализация
        const init = () => {
            // События аудио
            audio.addEventListener('timeupdate', updateProgress);
            audio.addEventListener('ended', () => {
                isPlaying = false;
                playPauseBtn.innerHTML = '▶';
                showStatus('Трек завершен');
            });
            
            // Обработчики UI
            progressBar.addEventListener('click', setProgress);
            volumeSlider.addEventListener('click', setVolume);
            playPauseBtn.addEventListener('click', togglePlay);
            loadUrlBtn.addEventListener('click', loadAudio);
            
            audioUrlInput.addEventListener('keypress', (e) => {
                if (e.key === 'Enter') loadAudio();
            });
            
            // Инициализация громкости
            volumeLevel.style.width = `${audio.volume * 100}%`;
            
            // Загрузить пример при запуске
            setTimeout(loadAudio, 500);
        };
        
        // Запуск
        init();
    </script>
</body>
</html>
