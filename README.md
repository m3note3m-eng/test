<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Telegram Аудиоплеер</title>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, sans-serif;
        }

        body {
            background-color: #1e1e1e;
            color: #e1e1e1;
            padding: 0;
            margin: 0;
            height: 100vh;
            overflow: hidden;
        }

        .container {
            display: flex;
            flex-direction: column;
            height: 100vh;
            max-width: 100%;
            background: linear-gradient(135deg, #1a1a2e 0%, #16213e 100%);
        }

        .header {
            background-color: #0f0f23;
            padding: 15px 20px;
            display: flex;
            justify-content: space-between;
            align-items: center;
            border-bottom: 1px solid #333;
            box-shadow: 0 2px 10px rgba(0, 0, 0, 0.3);
        }

        .logo {
            display: flex;
            align-items: center;
            gap: 10px;
        }

        .logo-icon {
            color: #34b7f1;
            font-size: 24px;
        }

        h1 {
            font-size: 1.4rem;
            color: #ffffff;
        }

        .player-section {
            padding: 25px 20px;
            display: flex;
            flex-direction: column;
            align-items: center;
            gap: 25px;
            flex-grow: 1;
        }

        .album-art {
            width: 220px;
            height: 220px;
            border-radius: 12px;
            overflow: hidden;
            box-shadow: 0 10px 25px rgba(0, 0, 0, 0.5);
            background: linear-gradient(135deg, #2a2a4e 0%, #1a1a3e 100%);
            display: flex;
            align-items: center;
            justify-content: center;
            color: #34b7f1;
            font-size: 60px;
        }

        .track-info {
            text-align: center;
            width: 100%;
            max-width: 400px;
        }

        .track-title {
            font-size: 1.6rem;
            font-weight: 600;
            margin-bottom: 5px;
            color: #ffffff;
            overflow: hidden;
            text-overflow: ellipsis;
            white-space: nowrap;
        }

        .track-artist {
            font-size: 1.1rem;
            color: #b0b0b0;
            margin-bottom: 20px;
        }

        .progress-container {
            width: 100%;
            max-width: 400px;
            margin-bottom: 5px;
        }

        .progress-bar {
            width: 100%;
            height: 6px;
            background-color: #333;
            border-radius: 3px;
            overflow: hidden;
            cursor: pointer;
        }

        .progress {
            height: 100%;
            background-color: #34b7f1;
            width: 0%;
            border-radius: 3px;
            transition: width 0.1s linear;
        }

        .time-info {
            display: flex;
            justify-content: space-between;
            width: 100%;
            max-width: 400px;
            font-size: 0.85rem;
            color: #888;
            margin-bottom: 25px;
        }

        .player-controls {
            display: flex;
            justify-content: center;
            align-items: center;
            gap: 25px;
            margin-bottom: 20px;
        }

        .control-btn {
            background: none;
            border: none;
            color: #ffffff;
            cursor: pointer;
            font-size: 20px;
            width: 50px;
            height: 50px;
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            transition: background-color 0.2s;
        }

        .control-btn:hover {
            background-color: rgba(255, 255, 255, 0.1);
        }

        .play-pause-btn {
            width: 70px;
            height: 70px;
            background-color: #34b7f1;
            font-size: 28px;
        }

        .play-pause-btn:hover {
            background-color: #2aa3d9;
        }

        .volume-container {
            display: flex;
            align-items: center;
            gap: 10px;
            width: 100%;
            max-width: 400px;
            margin-bottom: 20px;
        }

        .volume-icon {
            color: #888;
            font-size: 18px;
        }

        .volume-slider {
            flex-grow: 1;
            height: 4px;
            -webkit-appearance: none;
            background: #333;
            border-radius: 2px;
            outline: none;
        }

        .volume-slider::-webkit-slider-thumb {
            -webkit-appearance: none;
            width: 16px;
            height: 16px;
            border-radius: 50%;
            background: #34b7f1;
            cursor: pointer;
        }

        .add-track-section {
            padding: 15px 20px;
            background-color: rgba(15, 15, 35, 0.8);
            border-top: 1px solid #333;
            border-bottom: 1px solid #333;
        }

        .input-group {
            display: flex;
            gap: 10px;
        }

        .url-input {
            flex-grow: 1;
            padding: 12px 15px;
            border: 1px solid #444;
            border-radius: 8px;
            background-color: #1e1e2e;
            color: #ffffff;
            font-size: 0.95rem;
            outline: none;
        }

        .url-input:focus {
            border-color: #34b7f1;
        }

        .add-btn {
            background-color: #34b7f1;
            color: white;
            border: none;
            border-radius: 8px;
            padding: 12px 20px;
            font-weight: 600;
            cursor: pointer;
            transition: background-color 0.2s;
        }

        .add-btn:hover {
            background-color: #2aa3d9;
        }

        .playlist-section {
            flex-grow: 1;
            overflow-y: auto;
            padding: 15px 20px;
            background-color: rgba(10, 10, 20, 0.7);
        }

        .playlist-title {
            font-size: 1.2rem;
            margin-bottom: 15px;
            color: #ffffff;
            display: flex;
            justify-content: space-between;
            align-items: center;
        }

        .playlist {
            list-style: none;
        }

        .playlist-item {
            padding: 12px 15px;
            border-bottom: 1px solid #333;
            display: flex;
            align-items: center;
            gap: 15px;
            cursor: pointer;
            transition: background-color 0.2s;
            border-radius: 8px;
            margin-bottom: 5px;
        }

        .playlist-item:hover {
            background-color: rgba(255, 255, 255, 0.05);
        }

        .playlist-item.active {
            background-color: rgba(52, 183, 241, 0.15);
            border-left: 3px solid #34b7f1;
        }

        .playlist-item-icon {
            color: #34b7f1;
            font-size: 18px;
        }

        .playlist-item-info {
            flex-grow: 1;
            overflow: hidden;
        }

        .playlist-item-title {
            font-weight: 500;
            color: #ffffff;
            margin-bottom: 3px;
            overflow: hidden;
            text-overflow: ellipsis;
            white-space: nowrap;
        }

        .playlist-item-url {
            font-size: 0.8rem;
            color: #888;
            overflow: hidden;
            text-overflow: ellipsis;
            white-space: nowrap;
        }

        .playlist-item-actions {
            display: flex;
            gap: 10px;
        }

        .action-btn {
            background: none;
            border: none;
            color: #888;
            cursor: pointer;
            font-size: 16px;
            transition: color 0.2s;
        }

        .action-btn:hover {
            color: #ffffff;
        }

        .delete-btn:hover {
            color: #ff5252;
        }

        .telegram-notice {
            text-align: center;
            padding: 10px;
            font-size: 0.85rem;
            color: #777;
            background-color: rgba(0, 0, 0, 0.2);
        }

        .empty-playlist {
            text-align: center;
            padding: 40px 20px;
            color: #666;
            font-style: italic;
        }

        @media (max-width: 768px) {
            .album-art {
                width: 180px;
                height: 180px;
                font-size: 50px;
            }
            
            .track-title {
                font-size: 1.4rem;
            }
            
            .play-pause-btn {
                width: 60px;
                height: 60px;
                font-size: 24px;
            }
        }

        @media (max-width: 480px) {
            .player-controls {
                gap: 15px;
            }
            
            .control-btn {
                width: 45px;
                height: 45px;
            }
            
            .album-art {
                width: 150px;
                height: 150px;
                font-size: 40px;
            }
        }
    </style>
</head>
<body>
    <div class="container">
        <div class="header">
            <div class="logo">
                <i class="fas fa-headphones logo-icon"></i>
                <h1>Telegram Audio Player</h1>
            </div>
            <div class="telegram-indicator">
                <i class="fab fa-telegram" style="color: #34b7f1;"></i>
            </div>
        </div>

        <div class="player-section">
            <div class="album-art">
                <i class="fas fa-music"></i>
            </div>
            
            <div class="track-info">
                <div class="track-title" id="currentTrackTitle">Выберите трек из плейлиста</div>
                <div class="track-artist" id="currentTrackArtist">Плеер готов к работе</div>
            </div>
            
            <div class="progress-container">
                <div class="progress-bar" id="progressBar">
                    <div class="progress" id="progress"></div>
                </div>
            </div>
            
            <div class="time-info">
                <span id="currentTime">0:00</span>
                <span id="duration">0:00</span>
            </div>
            
            <div class="player-controls">
                <button class="control-btn" id="prevBtn">
                    <i class="fas fa-step-backward"></i>
                </button>
                <button class="control-btn play-pause-btn" id="playPauseBtn">
                    <i class="fas fa-play"></i>
                </button>
                <button class="control-btn" id="nextBtn">
                    <i class="fas fa-step-forward"></i>
                </button>
            </div>
            
            <div class="volume-container">
                <i class="fas fa-volume-down volume-icon"></i>
                <input type="range" min="0" max="100" value="70" class="volume-slider" id="volumeSlider">
                <i class="fas fa-volume-up volume-icon"></i>
            </div>
        </div>

        <div class="add-track-section">
            <div class="input-group">
                <input type="text" class="url-input" id="urlInput" placeholder="Введите URL аудио потока (MP3, OGG, M3U8)">
                <button class="add-btn" id="addBtn">
                    <i class="fas fa-plus"></i> Добавить
                </button>
            </div>
        </div>

        <div class="playlist-section">
            <div class="playlist-title">
                <span>Плейлист</span>
                <span id="playlistCount">0 треков</span>
            </div>
            
            <ul class="playlist" id="playlist">
                <li class="empty-playlist">Плейлист пуст. Добавьте треки, используя поле выше.</li>
            </ul>
        </div>
        
        <div class="telegram-notice">
            Это приложение использует интеграцию с Telegram Web App API для лучшего взаимодействия
        </div>
    </div>

    <script>
        // Имитация интеграции с Telegram Web App API
        // В реальном приложении здесь был бы код инициализации Telegram Web App
        
        // Состояние приложения
        const state = {
            currentTrackIndex: -1,
            isPlaying: false,
            playlist: [],
            audio: null
        };

        // Элементы DOM
        const audioPlayer = new Audio();
        state.audio = audioPlayer;
        
        const playPauseBtn = document.getElementById('playPauseBtn');
        const prevBtn = document.getElementById('prevBtn');
        const nextBtn = document.getElementById('nextBtn');
        const progressBar = document.getElementById('progressBar');
        const progress = document.getElementById('progress');
        const currentTimeEl = document.getElementById('currentTime');
        const durationEl = document.getElementById('duration');
        const volumeSlider = document.getElementById('volumeSlider');
        const urlInput = document.getElementById('urlInput');
        const addBtn = document.getElementById('addBtn');
        const playlistEl = document.getElementById('playlist');
        const playlistCount = document.getElementById('playlistCount');
        const currentTrackTitle = document.getElementById('currentTrackTitle');
        const currentTrackArtist = document.getElementById('currentTrackArtist');

        // Предустановленные треки для демонстрации
        const demoPlaylist = [
            {
                title: "Lo-Fi Chill Vibes",
                artist: "Chillhop Music",
                url: "https://stream.zeno.fm/f3wvbbqmdg8uv"
            },
            {
                title: "Space Ambient",
                artist: "Space Dreams",
                url: "https://stream.zeno.fm/0r0xa792kwzuv"
            },
            {
                title: "Jazz Relaxation",
                artist: "Coffee Jazz",
                url: "https://stream.zeno.fm/f3wvbbqmdg8uv"
            }
        ];

        // Инициализация приложения
        function initApp() {
            // Загрузка плейлиста из localStorage или использование демо
            const savedPlaylist = localStorage.getItem('telegramAudioPlayerPlaylist');
            if (savedPlaylist) {
                state.playlist = JSON.parse(savedPlaylist);
            } else {
                state.playlist = [...demoPlaylist];
                savePlaylist();
            }
            
            renderPlaylist();
            updatePlaylistCount();
            
            // Установка обработчиков событий
            playPauseBtn.addEventListener('click', togglePlayPause);
            prevBtn.addEventListener('click', playPrevTrack);
            nextBtn.addEventListener('click', playNextTrack);
            progressBar.addEventListener('click', seek);
            volumeSlider.addEventListener('input', updateVolume);
            addBtn.addEventListener('click', addTrackFromInput);
            urlInput.addEventListener('keypress', (e) => {
                if (e.key === 'Enter') addTrackFromInput();
            });
            
            // Обработчики событий аудио
            audioPlayer.addEventListener('timeupdate', updateProgress);
            audioPlayer.addEventListener('loadedmetadata', updateDuration);
            audioPlayer.addEventListener('ended', playNextTrack);
            audioPlayer.addEventListener('play', () => {
                state.isPlaying = true;
                updatePlayButton();
            });
            audioPlayer.addEventListener('pause', () => {
                state.isPlaying = false;
                updatePlayButton();
            });
            
            // Установка громкости
            audioPlayer.volume = volumeSlider.value / 100;
            
            // Имитация Telegram Web App API
            simulateTelegramIntegration();
        }

        // Имитация Telegram Web App API
        function simulateTelegramIntegration() {
            console.log("Telegram Web App API инициализирован");
            
            // В реальном приложении здесь был бы код для связи с Telegram
            // Например:
            // window.Telegram.WebApp.ready();
            // window.Telegram.WebApp.expand();
        }

        // Воспроизведение/пауза
        function togglePlayPause() {
            if (state.currentTrackIndex === -1 && state.playlist.length > 0) {
                playTrack(0);
                return;
            }
            
            if (state.isPlaying) {
                audioPlayer.pause();
            } else {
                audioPlayer.play().catch(e => {
                    console.error("Ошибка воспроизведения:", e);
                    alert("Не удалось воспроизвести аудио. Проверьте URL.");
                });
            }
        }

        // Воспроизведение трека
        function playTrack(index) {
            if (index < 0 || index >= state.playlist.length) return;
            
            state.currentTrackIndex = index;
            const track = state.playlist[index];
            
            // Обновление информации о треке
            currentTrackTitle.textContent = track.title;
            currentTrackArtist.textContent = track.artist;
            
            // Установка источника аудио
            audioPlayer.src = track.url;
            
            // Воспроизведение
            audioPlayer.play().catch(e => {
                console.error("Ошибка воспроизведения:", e);
                alert("Не удалось воспроизвести аудио. Проверьте URL.");
            });
            
            // Обновление активного элемента в плейлисте
            updateActivePlaylistItem();
            
            // Сохранение последнего воспроизводимого трека
            localStorage.setItem('telegramAudioPlayerLastTrack', index);
        }

        // Следующий трек
        function playNextTrack() {
            if (state.playlist.length === 0) return;
            
            let nextIndex = state.currentTrackIndex + 1;
            if (nextIndex >= state.playlist.length) {
                nextIndex = 0; // Вернуться к началу плейлиста
            }
            
            playTrack(nextIndex);
        }

        // Предыдущий трек
        function playPrevTrack() {
            if (state.playlist.length === 0) return;
            
            let prevIndex = state.currentTrackIndex - 1;
            if (prevIndex < 0) {
                prevIndex = state.playlist.length - 1; // Перейти к концу плейлиста
            }
            
            playTrack(prevIndex);
        }

        // Обновление кнопки воспроизведения/паузы
        function updatePlayButton() {
            const icon = playPauseBtn.querySelector('i');
            if (state.isPlaying) {
                icon.classList.remove('fa-play');
                icon.classList.add('fa-pause');
            } else {
                icon.classList.remove('fa-pause');
                icon.classList.add('fa-play');
            }
        }

        // Обновление прогресса
        function updateProgress() {
            if (!audioPlayer.duration) return;
            
            const percent = (audioPlayer.currentTime / audioPlayer.duration) * 100;
            progress.style.width = `${percent}%`;
            
            // Обновление времени
            currentTimeEl.textContent = formatTime(audioPlayer.currentTime);
        }

        // Обновление длительности
        function updateDuration() {
            durationEl.textContent = formatTime(audioPlayer.duration);
        }

        // Перемотка
        function seek(e) {
            const rect = progressBar.getBoundingClientRect();
            const percent = (e.clientX - rect.left) / rect.width;
            audioPlayer.currentTime = percent * audioPlayer.duration;
        }

        // Обновление громкости
        function updateVolume() {
            audioPlayer.volume = volumeSlider.value / 100;
        }

        // Форматирование времени
        function formatTime(seconds) {
            if (isNaN(seconds)) return "0:00";
            
            const mins = Math.floor(seconds / 60);
            const secs = Math.floor(seconds % 60);
            return `${mins}:${secs < 10 ? '0' : ''}${secs}`;
        }

        // Добавление трека
        function addTrackFromInput() {
            const url = urlInput.value.trim();
            if (!url) {
                alert("Пожалуйста, введите URL аудио");
                return;
            }
            
            // Проверка URL
            if (!isValidUrl(url)) {
                alert("Пожалуйста, введите корректный URL");
                return;
            }
            
            // Создание нового трека
            const newTrack = {
                title: `Трек ${state.playlist.length + 1}`,
                artist: "Неизвестный исполнитель",
                url: url
            };
            
            // Попытка получить метаданные из URL
            extractTrackInfoFromUrl(url, newTrack);
            
            // Добавление в плейлист
            state.playlist.push(newTrack);
            savePlaylist();
            renderPlaylist();
            updatePlaylistCount();
            
            // Очистка поля ввода
            urlInput.value = '';
            
            // Автовоспроизведение, если это первый трек
            if (state.playlist.length === 1 && state.currentTrackIndex === -1) {
                playTrack(0);
            }
        }

        // Извлечение информации о треке из URL
        function extractTrackInfoFromUrl(url, track) {
            try {
                const urlObj = new URL(url);
                const pathParts = urlObj.pathname.split('/').filter(part => part);
                
                if (pathParts.length > 0) {
                    const lastPart = pathParts[pathParts.length - 1];
                    // Удаление расширения файла
                    const nameWithoutExt = lastPart.replace(/\.[^/.]+$/, "");
                    if (nameWithoutExt) {
                        track.title = decodeURIComponent(nameWithoutExt).replace(/[+_]/g, ' ');
                    }
                }
                
                // Если это zeno.fm поток, попробуем извлечь информацию
                if (url.includes('zeno.fm')) {
                    track.artist = "Zeno.FM Radio";
                }
            } catch (e) {
                console.error("Ошибка при разборе URL:", e);
            }
        }

        // Проверка URL
        function isValidUrl(string) {
            try {
                new URL(string);
                return true;
            } catch (_) {
                return false;
            }
        }

        // Отображение плейлиста
        function renderPlaylist() {
            if (state.playlist.length === 0) {
                playlistEl.innerHTML = '<li class="empty-playlist">Плейлист пуст. Добавьте треки, используя поле выше.</li>';
                return;
            }
            
            playlistEl.innerHTML = '';
            
            state.playlist.forEach((track, index) => {
                const li = document.createElement('li');
                li.className = `playlist-item ${index === state.currentTrackIndex ? 'active' : ''}`;
                li.dataset.index = index;
                
                li.innerHTML = `
                    <i class="fas fa-music playlist-item-icon"></i>
                    <div class="playlist-item-info">
                        <div class="playlist-item-title">${track.title}</div>
                        <div class="playlist-item-url">${track.artist} • ${truncateUrl(track.url)}</div>
                    </div>
                    <div class="playlist-item-actions">
                        <button class="action-btn play-btn" data-index="${index}">
                            <i class="fas ${index === state.currentTrackIndex && state.isPlaying ? 'fa-pause' : 'fa-play'}"></i>
                        </button>
                        <button class="action-btn delete-btn" data-index="${index}">
                            <i class="fas fa-trash"></i>
                        </button>
                    </div>
                `;
                
                playlistEl.appendChild(li);
                
                // Обработчики событий для элементов плейлиста
                li.addEventListener('click', (e) => {
                    if (!e.target.closest('.playlist-item-actions')) {
                        playTrack(index);
                    }
                });
                
                // Обработчик для кнопки воспроизведения/паузы в плейлисте
                const playBtn = li.querySelector('.play-btn');
                playBtn.addEventListener('click', (e) => {
                    e.stopPropagation();
                    if (index === state.currentTrackIndex) {
                        togglePlayPause();
                    } else {
                        playTrack(index);
                    }
                });
                
                // Обработчик для кнопки удаления
                const deleteBtn = li.querySelector('.delete-btn');
                deleteBtn.addEventListener('click', (e) => {
                    e.stopPropagation();
                    deleteTrack(index);
                });
            });
            
            // Восстановление последнего воспроизводимого трека
            const lastTrackIndex = localStorage.getItem('telegramAudioPlayerLastTrack');
            if (lastTrackIndex && !state.currentTrackIndex >= 0) {
                const index = parseInt(lastTrackIndex);
                if (index >= 0 && index < state.playlist.length) {
                    // Только загружаем трек, но не воспроизводим автоматически
                    state.currentTrackIndex = index;
                    const track = state.playlist[index];
                    currentTrackTitle.textContent = track.title;
                    currentTrackArtist.textContent = track.artist;
                    updateActivePlaylistItem();
                }
            }
        }

        // Обновление активного элемента плейлиста
        function updateActivePlaylistItem() {
            document.querySelectorAll('.playlist-item').forEach((item, index) => {
                if (index === state.currentTrackIndex) {
                    item.classList.add('active');
                    const playIcon = item.querySelector('.play-btn i');
                    if (playIcon) {
                        playIcon.className = state.isPlaying ? 'fas fa-pause' : 'fas fa-play';
                    }
                } else {
                    item.classList.remove('active');
                    const playIcon = item.querySelector('.play-btn i');
                    if (playIcon) {
                        playIcon.className = 'fas fa-play';
                    }
                }
            });
        }

        // Удаление трека
        function deleteTrack(index) {
            if (confirm("Удалить этот трек из плейлиста?")) {
                // Если удаляем текущий трек, останавливаем воспроизведение
                if (index === state.currentTrackIndex) {
                    audioPlayer.pause();
                    state.currentTrackIndex = -1;
                    currentTrackTitle.textContent = "Выберите трек из плейлиста";
                    currentTrackArtist.textContent = "Плеер готов к работе";
                }
                
                // Удаление трека из плейлиста
                state.playlist.splice(index, 1);
                
                // Корректировка индекса текущего трека
                if (state.currentTrackIndex >= index && state.currentTrackIndex > 0) {
                    state.currentTrackIndex--;
                }
                
                savePlaylist();
                renderPlaylist();
                updatePlaylistCount();
            }
        }

        // Обновление счетчика треков
        function updatePlaylistCount() {
            const count = state.playlist.length;
            playlistCount.textContent = `${count} ${pluralize(count, ['трек', 'трека', 'треков'])}`;
        }

        // Функция для склонения слов
        function pluralize(number, words) {
            const cases = [2, 0, 1, 1, 1, 2];
            return words[(number % 100 > 4 && number % 100 < 20) ? 2 : cases[Math.min(number % 10, 5)]];
        }

        // Обрезка URL для отображения
        function truncateUrl(url, maxLength = 40) {
            if (url.length <= maxLength) return url;
            return url.substring(0, maxLength - 3) + '...';
        }

        // Сохранение плейлиста
        function savePlaylist() {
            localStorage.setItem('telegramAudioPlayerPlaylist', JSON.stringify(state.playlist));
        }

        // Инициализация приложения после загрузки DOM
        document.addEventListener('DOMContentLoaded', initApp);
    </script>
</body>
</html>
