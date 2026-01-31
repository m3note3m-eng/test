<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Аудиоплеер Telegram Mini App</title>
    <script src="https://telegram.org/js/telegram-web-app.js"></script>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, sans-serif;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            color: white;
            min-height: 100vh;
            padding: 20px;
        }

        .container {
            max-width: 500px;
            margin: 0 auto;
            background: rgba(255, 255, 255, 0.1);
            backdrop-filter: blur(10px);
            border-radius: 20px;
            padding: 20px;
            box-shadow: 0 8px 32px rgba(0, 0, 0, 0.3);
        }

        header {
            text-align: center;
            margin-bottom: 30px;
            padding-bottom: 20px;
            border-bottom: 2px solid rgba(255, 255, 255, 0.2);
        }

        header h1 {
            font-size: 2rem;
            margin-bottom: 10px;
        }

        .player-container {
            background: rgba(0, 0, 0, 0.3);
            border-radius: 15px;
            padding: 20px;
            margin-bottom: 25px;
        }

        .current-track {
            display: flex;
            align-items: center;
            margin-bottom: 20px;
            gap: 15px;
        }

        #track-cover {
            width: 80px;
            height: 80px;
            border-radius: 10px;
            object-fit: cover;
        }

        .track-info h2 {
            font-size: 1.2rem;
            margin-bottom: 5px;
            white-space: nowrap;
            overflow: hidden;
            text-overflow: ellipsis;
            max-width: 250px;
        }

        .track-info p {
            color: rgba(255, 255, 255, 0.8);
            font-size: 0.9rem;
        }

        .progress-container {
            margin-bottom: 20px;
        }

        .time-display {
            display: flex;
            justify-content: space-between;
            margin-bottom: 5px;
            font-size: 0.9rem;
            color: rgba(255, 255, 255, 0.9);
        }

        #progress-bar {
            width: 100%;
            height: 6px;
            border-radius: 3px;
            background: rgba(255, 255, 255, 0.2);
            outline: none;
            -webkit-appearance: none;
        }

        #progress-bar::-webkit-slider-thumb {
            -webkit-appearance: none;
            width: 20px;
            height: 20px;
            border-radius: 50%;
            background: #667eea;
            cursor: pointer;
        }

        .controls {
            display: flex;
            justify-content: center;
            align-items: center;
            gap: 30px;
            margin-bottom: 20px;
        }

        .control-btn {
            background: none;
            border: none;
            color: white;
            font-size: 2rem;
            cursor: pointer;
            padding: 10px;
            border-radius: 50%;
            transition: all 0.3s ease;
        }

        .control-btn:hover {
            background: rgba(255, 255, 255, 0.1);
        }

        .play-btn {
            background: #667eea;
            width: 60px;
            height: 60px;
            display: flex;
            align-items: center;
            justify-content: center;
        }

        .volume-container {
            display: flex;
            align-items: center;
            gap: 10px;
            margin-bottom: 20px;
            padding: 0 10px;
        }

        #volume-slider {
            flex-grow: 1;
            height: 6px;
            border-radius: 3px;
            background: rgba(255, 255, 255, 0.2);
            outline: none;
            -webkit-appearance: none;
        }

        .playback-controls {
            display: flex;
            justify-content: center;
            gap: 20px;
        }

        .icon-btn {
            background: rgba(255, 255, 255, 0.1);
            border: none;
            color: white;
            font-size: 1.2rem;
            padding: 10px 15px;
            border-radius: 10px;
            cursor: pointer;
            transition: all 0.3s ease;
        }

        .icon-btn:hover {
            background: rgba(255, 255, 255, 0.2);
        }

        .playlist-container {
            background: rgba(0, 0, 0, 0.3);
            border-radius: 15px;
            padding: 20px;
            margin-bottom: 25px;
        }

        .playlist-container h3 {
            margin-bottom: 15px;
            font-size: 1.2rem;
        }

        .add-stream-form {
            display: flex;
            flex-direction: column;
            gap: 10px;
            margin-bottom: 20px;
        }

        .add-stream-form input {
            padding: 12px;
            border: none;
            border-radius: 8px;
            background: rgba(255, 255, 255, 0.1);
            color: white;
            font-size: 14px;
        }

        .add-stream-form input::placeholder {
            color: rgba(255, 255, 255, 0.6);
        }

        #add-btn {
            padding: 12px;
            background: #667eea;
            color: white;
            border: none;
            border-radius: 8px;
            cursor: pointer;
            font-weight: bold;
            transition: background 0.3s ease;
        }

        #add-btn:hover {
            background: #5a67d8;
        }

        #playlist {
            list-style: none;
            max-height: 300px;
            overflow-y: auto;
        }

        .playlist-item {
            display: flex;
            justify-content: space-between;
            align-items: center;
            padding: 12px 15px;
            background: rgba(255, 255, 255, 0.1);
            border-radius: 10px;
            margin-bottom: 10px;
            cursor: pointer;
            transition: all 0.3s ease;
        }

        .playlist-item:hover {
            background: rgba(255, 255, 255, 0.15);
        }

        .playlist-item.active {
            background: rgba(102, 126, 234, 0.3);
            border-left: 4px solid #667eea;
        }

        .playlist-item .title {
            font-weight: 500;
            margin-bottom: 5px;
        }

        .playlist-item .url {
            font-size: 0.8rem;
            color: rgba(255, 255, 255, 0.6);
            white-space: nowrap;
            overflow: hidden;
            text-overflow: ellipsis;
            max-width: 200px;
        }

        .delete-btn {
            background: rgba(255, 59, 48, 0.2);
            color: #ff3b30;
            border: none;
            border-radius: 5px;
            padding: 5px 10px;
            cursor: pointer;
            font-size: 0.9rem;
        }

        .sample-tracks {
            background: rgba(0, 0, 0, 0.3);
            border-radius: 15px;
            padding: 20px;
        }

        .sample-tracks h3 {
            margin-bottom: 15px;
            font-size: 1.2rem;
        }

        .sample-buttons {
            display: flex;
            flex-direction: column;
            gap: 10px;
        }

        .sample-btn {
            padding: 12px;
            background: rgba(255, 255, 255, 0.1);
            border: none;
            border-radius: 10px;
            color: white;
            font-size: 14px;
            cursor: pointer;
            text-align: left;
            transition: all 0.3s ease;
        }

        .sample-btn:hover {
            background: rgba(255, 255, 255, 0.15);
        }

        /* Скроллбар */
        ::-webkit-scrollbar {
            width: 6px;
        }

        ::-webkit-scrollbar-track {
            background: rgba(255, 255, 255, 0.1);
            border-radius: 3px;
        }

        ::-webkit-scrollbar-thumb {
            background: rgba(255, 255, 255, 0.3);
            border-radius: 3px;
        }

        /* Адаптивность */
        @media (max-width: 480px) {
            .container {
                padding: 15px;
            }
            
            header h1 {
                font-size: 1.5rem;
            }
            
            .controls {
                gap: 20px;
            }
            
            .control-btn {
                font-size: 1.5rem;
            }
            
            .play-btn {
                width: 50px;
                height: 50px;
            }
        }
    </style>
</head>
<body>
    <div class="container">
        <header>
            <h1>🎵 Аудиоплеер</h1>
            <p>Воспроизведение в фоне и потоковое аудио</p>
        </header>

        <div class="player-container">
            <div class="current-track">
                <img id="track-cover" src="https://via.placeholder.com/300x300/4A6572/FFFFFF?text=Cover" alt="Обложка">
                <div class="track-info">
                    <h2 id="track-title">Выберите трек</h2>
                    <p id="track-artist">-</p>
                </div>
            </div>

            <div class="progress-container">
                <div class="time-display">
                    <span id="current-time">0:00</span>
                    <span id="total-time">0:00</span>
                </div>
                <input type="range" id="progress-bar" min="0" max="100" value="0">
            </div>

            <div class="controls">
                <button id="prev-btn" class="control-btn">⏮</button>
                <button id="play-btn" class="control-btn play-btn">▶</button>
                <button id="next-btn" class="control-btn">⏭</button>
            </div>

            <div class="volume-container">
                <span>🔈</span>
                <input type="range" id="volume-slider" min="0" max="100" value="50">
                <span>🔊</span>
            </div>

            <div class="playback-controls">
                <button id="repeat-btn" class="icon-btn" title="Повтор">🔁</button>
                <button id="shuffle-btn" class="icon-btn" title="Перемешать">🔀</button>
                <button id="add-stream-btn" class="icon-btn" title="Добавить поток">➕</button>
            </div>
        </div>

        <div class="playlist-container">
            <h3>Плейлист</h3>
            <div class="add-stream-form">
                <input type="text" id="stream-url" placeholder="Вставьте URL аудиопотока">
                <input type="text" id="stream-name" placeholder="Название трека">
                <button id="add-btn">Добавить</button>
            </div>
            <ul id="playlist"></ul>
        </div>

        <div class="sample-tracks">
            <h3>Примеры потоков:</h3>
            <div class="sample-buttons">
                <button class="sample-btn" data-url="https://stream.zeno.fm/0r0xa792kwzuv" data-name="Lofi Radio">🎧 Lofi Radio</button>
                <button class="sample-btn" data-url="https://stream.zeno.fm/f3wvbbqmdg8uv" data-name="Chillhop">🎵 Chillhop</button>
                <button class="sample-btn" data-url="https://radio.streemlion.com:1750/stream" data-name="Synthwave">📻 Synthwave</button>
                <button class="sample-btn" data-url="https://icecast.radiofrance.fr/fip-midfi.mp3" data-name="Radio France">🇫🇷 Radio France</button>
            </div>
        </div>
    </div>

    <script>
        // Инициализация Telegram Web App
        const tg = window.Telegram.WebApp;
        tg.expand();

        // Состояние приложения
        const state = {
            audio: new Audio(),
            playlist: [],
            currentTrackIndex: -1,
            isPlaying: false,
            isShuffle: false,
            repeatMode: 'none', // 'none', 'all', 'one'
            volume: 0.5
        };

        // DOM элементы
        const elements = {
            playBtn: document.getElementById('play-btn'),
            prevBtn: document.getElementById('prev-btn'),
            nextBtn: document.getElementById('next-btn'),
            progressBar: document.getElementById('progress-bar'),
            currentTime: document.getElementById('current-time'),
            totalTime: document.getElementById('total-time'),
            volumeSlider: document.getElementById('volume-slider'),
            repeatBtn: document.getElementById('repeat-btn'),
            shuffleBtn: document.getElementById('shuffle-btn'),
            addStreamBtn: document.getElementById('add-stream-btn'),
            addBtn: document.getElementById('add-btn'),
            streamUrl: document.getElementById('stream-url'),
            streamName: document.getElementById('stream-name'),
            playlist: document.getElementById('playlist'),
            trackTitle: document.getElementById('track-title'),
            trackArtist: document.getElementById('track-artist'),
            trackCover: document.getElementById('track-cover')
        };

        // Инициализация аудио
        state.audio.volume = state.volume;
        state.audio.crossOrigin = "anonymous";

        // События аудио
        state.audio.addEventListener('timeupdate', updateProgress);
        state.audio.addEventListener('loadedmetadata', updateTrackInfo);
        state.audio.addEventListener('ended', handleTrackEnd);
        state.audio.addEventListener('error', handleAudioError);

        // Управление воспроизведением
        elements.playBtn.addEventListener('click', togglePlay);
        elements.prevBtn.addEventListener('click', playPrev);
        elements.nextBtn.addEventListener('click', playNext);
        elements.progressBar.addEventListener('input', seek);
        elements.volumeSlider.addEventListener('input', changeVolume);
        elements.repeatBtn.addEventListener('click', toggleRepeat);
        elements.shuffleBtn.addEventListener('click', toggleShuffle);
        elements.addBtn.addEventListener('click', addStream);
        elements.addStreamBtn.addEventListener('click', () => {
            elements.streamUrl.focus();
        });

        // Обработчики для примеров потоков
        document.querySelectorAll('.sample-btn').forEach(btn => {
            btn.addEventListener('click', () => {
                const url = btn.dataset.url;
                const name = btn.dataset.name;
                addToPlaylist(name, url);
            });
        });

        // Функции управления
        function togglePlay() {
            if (state.currentTrackIndex === -1 && state.playlist.length > 0) {
                playTrack(0);
            } else if (state.isPlaying) {
                pause();
            } else {
                play();
            }
        }

        function play() {
            if (state.currentTrackIndex !== -1) {
                state.audio.play()
                    .then(() => {
                        state.isPlaying = true;
                        updatePlayButton();
                        updateTelegramPlayer();
                    })
                    .catch(handlePlayError);
            }
        }

        function pause() {
            state.audio.pause();
            state.isPlaying = false;
            updatePlayButton();
            updateTelegramPlayer();
        }

        function playTrack(index) {
            if (state.playlist.length === 0) return;
            
            state.currentTrackIndex = index;
            const track = state.playlist[index];
            
            state.audio.src = track.url;
            state.audio.load();
            
            updateTrackDisplay(track);
            updatePlaylistDisplay();
            
            play();
        }

        function playPrev() {
            if (state.playlist.length === 0) return;
            
            let newIndex = state.currentTrackIndex - 1;
            if (newIndex < 0) {
                newIndex = state.playlist.length - 1;
            }
            
            playTrack(newIndex);
        }

        function playNext() {
            if (state.playlist.length === 0) return;
            
            let newIndex = state.currentTrackIndex + 1;
            if (newIndex >= state.playlist.length) {
                newIndex = 0;
            }
            
            playTrack(newIndex);
        }

        // Функции отображения
        function updatePlayButton() {
            elements.playBtn.textContent = state.isPlaying ? '⏸' : '▶';
        }

        function updateProgress() {
            if (state.audio.duration) {
                const progress = (state.audio.currentTime / state.audio.duration) * 100;
                elements.progressBar.value = progress;
                elements.currentTime.textContent = formatTime(state.audio.currentTime);
            }
        }

        function updateTrackInfo() {
            elements.totalTime.textContent = formatTime(state.audio.duration);
        }

        function updateTrackDisplay(track) {
            elements.trackTitle.textContent = track.title;
            elements.trackArtist.textContent = track.url;
            elements.trackCover.src = `https://via.placeholder.com/300x300/4A6572/FFFFFF?text=${encodeURIComponent(track.title.substring(0, 10))}`;
        }

        function updatePlaylistDisplay() {
            elements.playlist.innerHTML = '';
            
            state.playlist.forEach((track, index) => {
                const li = document.createElement('li');
                li.className = `playlist-item ${index === state.currentTrackIndex ? 'active' : ''}`;
                
                li.innerHTML = `
                    <div>
                        <div class="title">${track.title}</div>
                        <div class="url">${track.url}</div>
                    </div>
                    <button class="delete-btn" data-index="${index}">✕</button>
                `;
                
                li.addEventListener('click', (e) => {
                    if (!e.target.classList.contains('delete-btn')) {
                        playTrack(index);
                    }
                });
                
                li.querySelector('.delete-btn').addEventListener('click', (e) => {
                    e.stopPropagation();
                    deleteTrack(index);
                });
                
                elements.playlist.appendChild(li);
            });
        }

        // Функции плейлиста
        function addToPlaylist(title, url) {
            const track = {
                id: Date.now(),
                title: title || `Трек ${state.playlist.length + 1}`,
                url: url,
                addedAt: new Date().toISOString()
            };
            
            state.playlist.push(track);
            updatePlaylistDisplay();
            savePlaylist();
            
            // Если это первый трек, начинаем воспроизведение
            if (state.currentTrackIndex === -1 && state.playlist.length === 1) {
                playTrack(0);
            }
            
            // Очистка полей ввода
            elements.streamUrl.value = '';
            elements.streamName.value = '';
        }

        function addStream() {
            const url = elements.streamUrl.value.trim();
            const name = elements.streamName.value.trim();
            
            if (!url) {
                showMessage('Введите URL потока');
                return;
            }
            
            if (!isValidUrl(url)) {
                showMessage('Введите корректный URL');
                return;
            }
            
            addToPlaylist(name, url);
            showMessage('Трек добавлен в плейлист');
        }

        function deleteTrack(index) {
            state.playlist.splice(index, 1);
            
            if (state.currentTrackIndex === index) {
                if (state.playlist.length > 0) {
                    playTrack(Math.min(index, state.playlist.length - 1));
                } else {
                    state.currentTrackIndex = -1;
                    state.audio.src = '';
                    updateTrackDisplay({ title: 'Выберите трек', url: '-' });
                    pause();
                }
            } else if (state.currentTrackIndex > index) {
                state.currentTrackIndex--;
            }
            
            updatePlaylistDisplay();
            savePlaylist();
        }

        // Функции управления режимами
        function toggleRepeat() {
            const modes = ['none', 'all', 'one'];
            const currentIndex = modes.indexOf(state.repeatMode);
            state.repeatMode = modes[(currentIndex + 1) % modes.length];
            
            elements.repeatBtn.style.opacity = state.repeatMode === 'none' ? '0.5' : '1';
            elements.repeatBtn.title = `Повтор: ${state.repeatMode}`;
        }

        function toggleShuffle() {
            state.isShuffle = !state.isShuffle;
            elements.shuffleBtn.style.opacity = state.isShuffle ? '1' : '0.5';
            
            if (state.isShuffle && state.playlist.length > 1) {
                shufflePlaylist();
            }
        }

        function shufflePlaylist() {
            const currentTrack = state.playlist[state.currentTrackIndex];
            
            for (let i = state.playlist.length - 1; i > 0; i--) {
                const j = Math.floor(Math.random() * (i + 1));
                [state.playlist[i], state.playlist[j]] = [state.playlist[j], state.playlist[i]];
            }
            
            // Перемещаем текущий трек на первую позицию
            if (currentTrack) {
                const newIndex = state.playlist.findIndex(t => t.id === currentTrack.id);
                if (newIndex > 0) {
                    [state.playlist[0], state.playlist[newIndex]] = [state.playlist[newIndex], state.playlist[0]];
                    state.currentTrackIndex = 0;
                }
            }
            
            updatePlaylistDisplay();
            savePlaylist();
        }

        // Обработчики событий
        function handleTrackEnd() {
            if (state.repeatMode === 'one') {
                state.audio.currentTime = 0;
                play();
            } else if (state.repeatMode === 'all' || state.currentTrackIndex < state.playlist.length - 1) {
                playNext();
            } else {
                pause();
            }
        }

        function handleAudioError(e) {
            console.error('Audio error:', e);
            showMessage('Ошибка воспроизведения аудио');
            
            // Пропускаем нерабочий трек
            if (state.isPlaying) {
                setTimeout(() => playNext(), 1000);
            }
        }

        function handlePlayError(error) {
            console.error('Play error:', error);
            showMessage('Ошибка при запуске воспроизведения');
        }

        // Вспомогательные функции
        function seek() {
            if (state.audio.duration) {
                const time = (elements.progressBar.value / 100) * state.audio.duration;
                state.audio.currentTime = time;
            }
        }

        function changeVolume() {
            state.volume = elements.volumeSlider.value / 100;
            state.audio.volume = state.volume;
        }

        function formatTime(seconds) {
            if (isNaN(seconds)) return '0:00';
            
            const mins = Math.floor(seconds / 60);
            const secs = Math.floor(seconds % 60);
            return `${mins}:${secs.toString().padStart(2, '0')}`;
        }

        function isValidUrl(string) {
            try {
                new URL(string);
                return true;
            } catch (_) {
                return false;
            }
        }

        function showMessage(text) {
            tg.showPopup({
                title: 'Уведомление',
                message: text,
                buttons: [{ type: 'ok' }]
            });
        }

        // Работа с Telegram API
        function updateTelegramPlayer() {
            if (!state.playlist[state.currentTrackIndex]) return;
            
            const track = state.playlist[state.currentTrackIndex];
            
            tg.MainButton.setText(state.isPlaying ? 'Пауза' : 'Играть');
            tg.MainButton.onClick(togglePlay);
            tg.MainButton.show();
            
            // Обновление состояния плеера в Telegram
            if (window.Telegram && window.Telegram.WebApp) {
                window.Telegram.WebApp.setHeaderColor('#667eea');
                
                // Отправка состояния для фонового воспроизведения
                try {
                    tg.sendData(JSON.stringify({
                        type: 'player_state',
                        isPlaying: state.isPlaying,
                        track: track.title,
                        progress: state.audio.currentTime,
                        duration: state.audio.duration
                    }));
                } catch (e) {
                    console.log('Telegram data send error:', e);
                }
            }
        }

        // Сохранение и загрузка плейлиста
        function savePlaylist() {
            localStorage.setItem('audioPlayer_playlist', JSON.stringify(state.playlist));
            localStorage.setItem('audioPlayer_currentIndex', state.currentTrackIndex);
        }

        function loadPlaylist() {
            const savedPlaylist = localStorage.getItem('audioPlayer_playlist');
            const savedIndex = localStorage.getItem('audioPlayer_currentIndex');
            
            if (savedPlaylist) {
                state.playlist = JSON.parse(savedPlaylist);
                state.currentTrackIndex = parseInt(savedIndex) || -1;
                
                if (state.currentTrackIndex >= 0 && state.currentTrackIndex < state.playlist.length) {
                    const track = state.playlist[state.currentTrackIndex];
                    state.audio.src = track.url;
                    updateTrackDisplay(track);
                }
                
                updatePlaylistDisplay();
            }
        }

        // Загрузка начальных данных
        function init() {
            // Загрузка сохраненного плейлиста
            loadPlaylist();
            
            // Установка начального состояния кнопок
            elements.volumeSlider.value = state.volume * 100;
            elements.repeatBtn.style.opacity = '0.5';
            elements.shuffleBtn.style.opacity = '0.5';
            
            // Инициализация Telegram Web App
            tg.ready();
            tg.setHeaderColor('#667eea');
            tg.MainButton.hide();
            
            // Добавляем несколько примеров при первом запуске
            if (state.playlist.length === 0) {
                addToPlaylist('Lofi Radio', 'https://stream.zeno.fm/0r0xa792kwzuv');
                addToPlaylist('Chillhop', 'https://stream.zeno.fm/f3wvbbqmdg8uv');
            }
            
            console.log('Audio player initialized');
        }

        // Запуск приложения
        document.addEventListener('DOMContentLoaded', init);

        // Service Worker для фонового воспроизведения
        if ('serviceWorker' in navigator) {
            window.addEventListener('load', () => {
                navigator.serviceWorker.register('/service-worker.js').then(registration => {
                    console.log('ServiceWorker registered:', registration);
                }).catch(error => {
                    console.log('ServiceWorker registration failed:', error);
                });
            });
        }

        // Глобальные функции для отладки
        window.playerState = state;
        window.debug = {
            playTrack,
            pause,
            play,
            addToPlaylist,
            getPlaylist: () => state.playlist
        };
    </script>

    <!-- Inline Service Worker код для фонового воспроизведения -->
    <script id="service-worker">
        // Этот код будет зарегистрирован как Service Worker при размещении на сервере
        const serviceWorkerCode = `
self.addEventListener('install', (event) => {
    console.log('Audio Player Service Worker installed');
    self.skipWaiting();
});

self.addEventListener('activate', (event) => {
    console.log('Audio Player Service Worker activated');
    event.waitUntil(clients.claim());
});

self.addEventListener('message', (event) => {
    if (event.data && event.data.type === 'PLAY_AUDIO') {
        console.log('Background playback request:', event.data);
        // Можно добавить логику для управления уведомлениями
    }
});

// Кэширование для офлайн работы
const CACHE_NAME = 'audio-player-v1';
const urlsToCache = [
    '/',
    '/index.html'
];

self.addEventListener('install', (event) => {
    event.waitUntil(
        caches.open(CACHE_NAME)
            .then(cache => cache.addAll(urlsToCache))
    );
});

self.addEventListener('fetch', (event) => {
    event.respondWith(
        caches.match(event.request)
            .then(response => response || fetch(event.request))
    );
});
`;
    </script>
</body>
</html>
