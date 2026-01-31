<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Telegram Audio Player</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            background: var(--tg-theme-bg-color, #1e2734);
            color: var(--tg-theme-text-color, #ffffff);
            font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;
            height: 100vh;
            overflow: hidden;
        }

        .player {
            height: 100%;
            display: flex;
            flex-direction: column;
            padding: 20px;
        }

        .header {
            padding: 10px 0;
            text-align: center;
            border-bottom: 1px solid rgba(255, 255, 255, 0.1);
            margin-bottom: 20px;
        }

        .track-info {
            flex: 1;
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            text-align: center;
            padding: 20px 0;
        }

        .track-title {
            font-size: 20px;
            font-weight: 600;
            margin-bottom: 5px;
            color: var(--tg-theme-text-color, #ffffff);
        }

        .track-artist {
            font-size: 14px;
            color: var(--tg-theme-hint-color, #8e8e93);
        }

        .progress-container {
            width: 100%;
            padding: 20px 0;
        }

        .progress-bar {
            width: 100%;
            height: 4px;
            background: rgba(255, 255, 255, 0.1);
            border-radius: 2px;
            margin-bottom: 8px;
            cursor: pointer;
            overflow: hidden;
        }

        .progress {
            height: 100%;
            background: var(--tg-theme-button-color, #2ea6ff);
            width: 0%;
            transition: width 0.1s linear;
        }

        .time-info {
            display: flex;
            justify-content: space-between;
            font-size: 12px;
            color: var(--tg-theme-hint-color, #8e8e93);
        }

        .controls {
            display: flex;
            justify-content: center;
            align-items: center;
            gap: 30px;
            padding: 20px 0;
        }

        .control-btn {
            background: none;
            border: none;
            color: var(--tg-theme-button-color, #2ea6ff);
            font-size: 20px;
            cursor: pointer;
            width: 44px;
            height: 44px;
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            transition: background-color 0.2s;
        }

        .control-btn:active {
            background-color: rgba(46, 166, 255, 0.1);
        }

        .play-btn {
            width: 60px;
            height: 60px;
            font-size: 24px;
            background: var(--tg-theme-button-color, #2ea6ff);
            color: var(--tg-theme-button-text-color, #ffffff);
        }

        .volume-container {
            display: flex;
            align-items: center;
            gap: 10px;
            padding: 15px 0;
        }

        .volume-slider {
            flex: 1;
            height: 4px;
            background: rgba(255, 255, 255, 0.1);
            border-radius: 2px;
            cursor: pointer;
        }

        .volume-level {
            height: 100%;
            background: var(--tg-theme-button-color, #2ea6ff);
            width: 70%;
        }

        .url-input {
            width: 100%;
            padding: 12px 16px;
            background: var(--tg-theme-secondary-bg-color, #2c3a4d);
            border: 1px solid rgba(255, 255, 255, 0.1);
            border-radius: 10px;
            color: var(--tg-theme-text-color, #ffffff);
            font-size: 14px;
            margin-bottom: 10px;
        }

        .url-input:focus {
            outline: none;
            border-color: var(--tg-theme-button-color, #2ea6ff);
        }

        .load-btn {
            width: 100%;
            padding: 12px;
            background: var(--tg-theme-button-color, #2ea6ff);
            color: var(--tg-theme-button-text-color, #ffffff);
            border: none;
            border-radius: 10px;
            font-size: 14px;
            font-weight: 500;
            cursor: pointer;
        }

        .status {
            text-align: center;
            font-size: 12px;
            color: var(--tg-theme-hint-color, #8e8e93);
            padding: 10px;
            min-height: 20px;
        }

        /* Telegram стилизация */
        .telegram-header {
            background: var(--tg-theme-bg-color, #1e2734);
            padding: 10px;
            position: sticky;
            top: 0;
            z-index: 100;
        }

        @media (prefers-color-scheme: light) {
            body {
                background: var(--tg-theme-bg-color, #f2f2f7);
                color: var(--tg-theme-text-color, #000000);
            }
            
            .url-input {
                background: var(--tg-theme-secondary-bg-color, #ffffff);
            }
        }
    </style>
</head>
<body>
    <div class="player">
        <div class="header">
            <h1 style="font-size: 18px;">🎵 Audio Player</h1>
        </div>
        
        <div class="track-info">
            <div class="track-title" id="trackTitle">Не выбрано</div>
            <div class="track-artist" id="trackArtist">Загрузите аудио по ссылке</div>
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
            <button class="control-btn" id="prevBtn">⏮</button>
            <button class="control-btn play-btn" id="playBtn">▶</button>
            <button class="control-btn" id="nextBtn">⏭</button>
        </div>
        
        <div class="volume-container">
            <span>🔈</span>
            <div class="volume-slider" id="volumeSlider">
                <div class="volume-level" id="volumeLevel"></div>
            </div>
            <span>🔊</span>
        </div>
        
        <div class="url-input-container">
            <input type="url" 
                   class="url-input" 
                   id="audioUrl" 
                   placeholder="Введите ссылку на аудио (MP3, M3U8, etc.)"
                   value="">
            <button class="load-btn" id="loadBtn">Загрузить аудио</button>
        </div>
        
        <div class="status" id="status"></div>
    </div>

    <script>
        // Telegram WebApp инициализация
        const tg = window.Telegram?.WebApp;
        
        if (tg) {
            tg.expand();
            tg.ready();
            tg.setHeaderColor('secondary_bg_color');
            tg.BackButton.hide();
            
            // Применяем тему Telegram
            const applyTheme = () => {
                document.documentElement.style.setProperty('--tg-theme-bg-color', tg.themeParams.bg_color || '#1e2734');
                document.documentElement.style.setProperty('--tg-theme-text-color', tg.themeParams.text_color || '#ffffff');
                document.documentElement.style.setProperty('--tg-theme-hint-color', tg.themeParams.hint_color || '#8e8e93');
                document.documentElement.style.setProperty('--tg-theme-button-color', tg.themeParams.button_color || '#2ea6ff');
                document.documentElement.style.setProperty('--tg-theme-button-text-color', tg.themeParams.button_text_color || '#ffffff');
                document.documentElement.style.setProperty('--tg-theme-secondary-bg-color', tg.themeParams.secondary_bg_color || '#2c3a4d');
            };
            
            applyTheme();
            
            // Обновляем тему при изменении
            tg.onEvent('themeChanged', applyTheme);
            
            // Отправляем данные в Telegram при изменении трека
            const sendPlayerState = () => {
                const state = {
                    type: 'player_state',
                    isPlaying: isPlaying,
                    currentTime: audio.currentTime,
                    duration: audio.duration,
                    track: trackTitle.textContent,
                    volume: audio.volume
                };
                tg.sendData(JSON.stringify(state));
            };
        }

        // Основной аудио элемент
        const audio = new Audio();
        audio.crossOrigin = "anonymous";
        
        // Элементы DOM
        const playBtn = document.getElementById('playBtn');
        const progressBar = document.getElementById('progressBar');
        const progress = document.getElementById('progress');
        const currentTimeEl = document.getElementById('currentTime');
        const durationEl = document.getElementById('duration');
        const volumeSlider = document.getElementById('volumeSlider');
        const volumeLevel = document.getElementById('volumeLevel');
        const audioUrlInput = document.getElementById('audioUrl');
        const loadBtn = document.getElementById('loadBtn');
        const trackTitle = document.getElementById('trackTitle');
        const trackArtist = document.getElementById('trackArtist');
        const statusEl = document.getElementById('status');
        
        // Состояние плеера
        let isPlaying = false;
        audio.volume = 0.7;
        volumeLevel.style.width = '70%';

        // Вспомогательные функции
        const formatTime = (seconds) => {
            const mins = Math.floor(seconds / 60);
            const secs = Math.floor(seconds % 60);
            return `${mins}:${secs.toString().padStart(2, '0')}`;
        };

        const updateProgress = () => {
            if (audio.duration) {
                const percent = (audio.currentTime / audio.duration) * 100;
                progress.style.width = `${percent}%`;
                currentTimeEl.textContent = formatTime(audio.currentTime);
            }
        };

        const setProgress = (e) => {
            const rect = progressBar.getBoundingClientRect();
            const percent = (e.clientX - rect.left) / rect.width;
            audio.currentTime = percent * audio.duration;
        };

        const setVolume = (e) => {
            const rect = volumeSlider.getBoundingClientRect();
            let percent = (e.clientX - rect.left) / rect.width;
            percent = Math.max(0, Math.min(1, percent));
            audio.volume = percent;
            volumeLevel.style.width = `${percent * 100}%`;
        };

        const togglePlay = () => {
            if (!audio.src) {
                showStatus('Сначала загрузите аудио');
                return;
            }

            if (isPlaying) {
                audio.pause();
                playBtn.textContent = '▶';
            } else {
                audio.play()
                    .then(() => {
                        playBtn.textContent = '⏸';
                        if (tg) sendPlayerState();
                    })
                    .catch(e => {
                        showStatus('Ошибка воспроизведения: ' + e.message);
                    });
            }
            isPlaying = !isPlaying;
        };

        const loadAudio = () => {
            const url = audioUrlInput.value.trim();
            if (!url) {
                showStatus('Введите ссылку на аудио');
                return;
            }

            showStatus('Загрузка...');
            
            // Проверяем, поддерживается ли формат
            if (!url.match(/\.(mp3|m3u8|aac|ogg|wav)$/i) && !url.includes('stream')) {
                showStatus('Формат может не поддерживаться');
            }

            audio.src = url;
            audio.load();

            audio.onloadedmetadata = () => {
                durationEl.textContent = formatTime(audio.duration);
                trackTitle.textContent = url.split('/').pop().split('?')[0] || 'Аудио поток';
                trackArtist.textContent = 'Прямая трансляция';
                showStatus('Загружено');
                if (tg) sendPlayerState();
            };

            audio.onerror = () => {
                showStatus('Ошибка загрузки аудио');
                audio.src = '';
            };
        };

        const showStatus = (message) => {
            statusEl.textContent = message;
            setTimeout(() => {
                if (statusEl.textContent === message) {
                    statusEl.textContent = '';
                }
            }, 3000);
        };

        // Инициализация событий
        const init = () => {
            // Аудио события
            audio.addEventListener('timeupdate', updateProgress);
            audio.addEventListener('ended', () => {
                isPlaying = false;
                playBtn.textContent = '▶';
                showStatus('Воспроизведение завершено');
                if (tg) sendPlayerState();
            });

            // UI события
            progressBar.addEventListener('click', setProgress);
            volumeSlider.addEventListener('click', setVolume);
            playBtn.addEventListener('click', togglePlay);
            loadBtn.addEventListener('click', loadAudio);
            
            audioUrlInput.addEventListener('keypress', (e) => {
                if (e.key === 'Enter') loadAudio();
            });

            // Предыдущий/следующий трек (базовая реализация)
            document.getElementById('prevBtn').addEventListener('click', () => {
                audio.currentTime = Math.max(0, audio.currentTime - 10);
            });

            document.getElementById('nextBtn').addEventListener('click', () => {
                audio.currentTime = Math.min(audio.duration, audio.currentTime + 10);
            });

            // Обработка ссылок из Telegram
            if (tg && tg.initDataUnsafe?.start_param) {
                const startParam = tg.initDataUnsafe.start_param;
                if (startParam.startsWith('http')) {
                    audioUrlInput.value = decodeURIComponent(startParam);
                    setTimeout(loadAudio, 500);
                }
            }
        };

        // Запуск приложения
        document.addEventListener('DOMContentLoaded', init);
    </script>
</body>
</html>
