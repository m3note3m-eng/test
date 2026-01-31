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

        .background-toggle {
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 10px;
            margin: 10px 0;
            padding: 10px;
            background: rgba(255, 255, 255, 0.05);
            border-radius: 10px;
        }

        .toggle-label {
            font-size: 14px;
        }

        .toggle-switch {
            position: relative;
            width: 50px;
            height: 24px;
        }

        .toggle-switch input {
            opacity: 0;
            width: 0;
            height: 0;
        }

        .toggle-slider {
            position: absolute;
            cursor: pointer;
            top: 0;
            left: 0;
            right: 0;
            bottom: 0;
            background-color: rgba(255, 255, 255, 0.2);
            transition: .4s;
            border-radius: 24px;
        }

        .toggle-slider:before {
            position: absolute;
            content: "";
            height: 16px;
            width: 16px;
            left: 4px;
            bottom: 4px;
            background-color: white;
            transition: .4s;
            border-radius: 50%;
        }

        input:checked + .toggle-slider {
            background-color: var(--tg-theme-button-color, #2ea6ff);
        }

        input:checked + .toggle-slider:before {
            transform: translateX(26px);
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
        
        <div class="background-toggle">
            <span class="toggle-label">Фоновое воспроизведение</span>
            <label class="toggle-switch">
                <input type="checkbox" id="backgroundToggle">
                <span class="toggle-slider"></span>
            </label>
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
        
        // Объект для управления медиасессией (для уведомлений и управления)
        let mediaSession = null;
        let audioContext = null;
        let sourceNode = null;
        
        // Инициализация Telegram WebApp
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
            tg.onEvent('themeChanged', applyTheme);
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
        const backgroundToggle = document.getElementById('backgroundToggle');
        
        // Состояние плеера
        let isPlaying = false;
        let enableBackgroundPlay = false;
        let currentTrack = {
            title: 'Не выбрано',
            artist: 'Загрузите аудио по ссылке',
            url: ''
        };
        
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

        // Инициализация Media Session API
        const initMediaSession = () => {
            if ('mediaSession' in navigator) {
                navigator.mediaSession.setActionHandler('play', togglePlay);
                navigator.mediaSession.setActionHandler('pause', togglePlay);
                navigator.mediaSession.setActionHandler('seekbackward', () => {
                    audio.currentTime = Math.max(0, audio.currentTime - 10);
                });
                navigator.mediaSession.setActionHandler('seekforward', () => {
                    audio.currentTime = Math.min(audio.duration, audio.currentTime + 10);
                });
                navigator.mediaSession.setActionHandler('previoustrack', () => {
                    audio.currentTime = 0;
                });
                
                updateMediaMetadata();
            }
        };

        const updateMediaMetadata = () => {
            if ('mediaSession' in navigator) {
                navigator.mediaSession.metadata = new MediaMetadata({
                    title: currentTrack.title,
                    artist: currentTrack.artist,
                    artwork: [
                        { src: 'https://via.placeholder.com/96x96', sizes: '96x96', type: 'image/png' },
                        { src: 'https://via.placeholder.com/128x128', sizes: '128x128', type: 'image/png' },
                        { src: 'https://via.placeholder.com/192x192', sizes: '192x192', type: 'image/png' },
                        { src: 'https://via.placeholder.com/256x256', sizes: '256x256', type: 'image/png' },
                        { src: 'https://via.placeholder.com/384x384', sizes: '384x384', type: 'image/png' },
                        { src: 'https://via.placeholder.com/512x512', sizes: '512x512', type: 'image/png' }
                    ]
                });
            }
        };

        // Фоновое воспроизведение с использованием AudioContext
        const setupBackgroundAudio = () => {
            if (!audioContext) {
                audioContext = new (window.AudioContext || window.webkitAudioContext)();
                
                // Восстанавливаем аудиоконтекст при пользовательском взаимодействии
                document.addEventListener('click', () => {
                    if (audioContext.state === 'suspended') {
                        audioContext.resume();
                    }
                }, { once: true });
            }
            
            if (sourceNode) {
                sourceNode.disconnect();
            }
            
            sourceNode = audioContext.createMediaElementSource(audio);
            sourceNode.connect(audioContext.destination);
        };

        const togglePlay = () => {
            if (!audio.src) {
                showStatus('Сначала загрузите аудио');
                return;
            }

            if (isPlaying) {
                audio.pause();
                playBtn.textContent = '▶';
                if ('mediaSession' in navigator) {
                    navigator.mediaSession.playbackState = 'paused';
                }
            } else {
                // Включаем фоновое воспроизведение если нужно
                if (enableBackgroundPlay && !audioContext) {
                    setupBackgroundAudio();
                }
                
                audio.play()
                    .then(() => {
                        playBtn.textContent = '⏸';
                        if ('mediaSession' in navigator) {
                            navigator.mediaSession.playbackState = 'playing';
                        }
                        
                        // Сохраняем состояние для восстановления
                        if (enableBackgroundPlay) {
                            localStorage.setItem('lastAudioState', JSON.stringify({
                                url: currentTrack.url,
                                time: audio.currentTime,
                                playing: true
                            }));
                        }
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
            if (!url.match(/\.(mp3|m3u8|aac|ogg|wav|flac)$/i) && !url.includes('stream')) {
                showStatus('Формат может не поддерживаться');
            }

            audio.src = url;
            audio.load();
            
            // Сохраняем информацию о треке
            const fileName = url.split('/').pop().split('?')[0] || 'Аудио поток';
            currentTrack = {
                title: decodeURIComponent(fileName.replace(/\.[^/.]+$/, "")) || 'Аудио поток',
                artist: 'Прямая трансляция',
                url: url
            };

            audio.onloadedmetadata = () => {
                durationEl.textContent = formatTime(audio.duration);
                trackTitle.textContent = currentTrack.title;
                trackArtist.textContent = currentTrack.artist;
                showStatus('Загружено');
                
                // Обновляем медиаметаданные
                updateMediaMetadata();
                
                // Включаем фоновое воспроизведение если нужно
                if (enableBackgroundPlay) {
                    setupBackgroundAudio();
                }
                
                // Инициализируем медиасессию
                initMediaSession();
                
                // Автовоспроизведение
                setTimeout(() => togglePlay(), 500);
            };

            audio.onerror = () => {
                showStatus('Ошибка загрузки аудио');
                audio.src = '';
            };
        };

        // Восстановление состояния при загрузке
        const restoreState = () => {
            try {
                const savedState = localStorage.getItem('lastAudioState');
                if (savedState) {
                    const state = JSON.parse(savedState);
                    if (state.url) {
                        audioUrlInput.value = state.url;
                        setTimeout(() => {
                            audio.addEventListener('loadedmetadata', () => {
                                if (state.time) {
                                    audio.currentTime = state.time;
                                }
                                if (state.playing) {
                                    setTimeout(() => togglePlay(), 100);
                                }
                            }, { once: true });
                            loadAudio();
                        }, 100);
                    }
                }
            } catch (e) {
                console.log('Не удалось восстановить состояние');
            }
        };

        const showStatus = (message) => {
            statusEl.textContent = message;
            setTimeout(() => {
                if (statusEl.textContent === message) {
                    statusEl.textContent = '';
                }
            }, 3000);
        };

        // Обработчики видимости страницы
        const handleVisibilityChange = () => {
            if (enableBackgroundPlay && audioContext && document.hidden) {
                // При скрытии страницы сохраняем состояние
                if (!audio.paused) {
                    localStorage.setItem('lastAudioState', JSON.stringify({
                        url: currentTrack.url,
                        time: audio.currentTime,
                        playing: true
                    }));
                }
            }
        };

        // Инициализация событий
        const init = () => {
            // Восстанавливаем состояние
            restoreState();
            
            // Аудио события
            audio.addEventListener('timeupdate', updateProgress);
            audio.addEventListener('ended', () => {
                isPlaying = false;
                playBtn.textContent = '▶';
                if ('mediaSession' in navigator) {
                    navigator.mediaSession.playbackState = 'paused';
                }
                showStatus('Воспроизведение завершено');
                
                // Очищаем сохраненное состояние
                localStorage.removeItem('lastAudioState');
            });

            // UI события
            progressBar.addEventListener('click', setProgress);
            volumeSlider.addEventListener('click', setVolume);
            playBtn.addEventListener('click', togglePlay);
            loadBtn.addEventListener('click', loadAudio);
            
            audioUrlInput.addEventListener('keypress', (e) => {
                if (e.key === 'Enter') loadAudio();
            });

            // Предыдущий/следующий трек
            document.getElementById('prevBtn').addEventListener('click', () => {
                audio.currentTime = Math.max(0, audio.currentTime - 10);
                if ('mediaSession' in navigator) {
                    navigator.mediaSession.setActionHandler('seekbackward', null);
                    setTimeout(() => {
                        navigator.mediaSession.setActionHandler('seekbackward', () => {
                            audio.currentTime = Math.max(0, audio.currentTime - 10);
                        });
                    }, 100);
                }
            });

            document.getElementById('nextBtn').addEventListener('click', () => {
                audio.currentTime = Math.min(audio.duration, audio.currentTime + 10);
                if ('mediaSession' in navigator) {
                    navigator.mediaSession.setActionHandler('seekforward', null);
                    setTimeout(() => {
                        navigator.mediaSession.setActionHandler('seekforward', () => {
                            audio.currentTime = Math.min(audio.duration, audio.currentTime + 10);
                        });
                    }, 100);
                }
            });

            // Переключатель фонового воспроизведения
            backgroundToggle.addEventListener('change', (e) => {
                enableBackgroundPlay = e.target.checked;
                if (enableBackgroundPlay && audio.src) {
                    setupBackgroundAudio();
                    showStatus('Фоновое воспроизведение включено');
                } else {
                    showStatus('Фоновое воспроизведение выключено');
                }
            });

            // Обработка видимости страницы
            document.addEventListener('visibilitychange', handleVisibilityChange);

            // Обработка ссылок из Telegram
            if (tg && tg.initDataUnsafe?.start_param) {
                const startParam = tg.initDataUnsafe.start_param;
                if (startParam.startsWith('http')) {
                    audioUrlInput.value = decodeURIComponent(startParam);
                    setTimeout(loadAudio, 500);
                }
            }
            
            // Автовключение фонового воспроизведения для мобильных устройств
            if (/Android|iPhone|iPad|iPod/i.test(navigator.userAgent)) {
                backgroundToggle.checked = true;
                enableBackgroundPlay = true;
            }
        };

        // Запуск приложения
        document.addEventListener('DOMContentLoaded', init);
        
        // Сохраняем состояние при закрытии
        window.addEventListener('beforeunload', () => {
            if (enableBackgroundPlay && audio.src && !audio.paused) {
                localStorage.setItem('lastAudioState', JSON.stringify({
                    url: currentTrack.url,
                    time: audio.currentTime,
                    playing: true
                }));
            }
        });
    </script>
</body>
</html>
