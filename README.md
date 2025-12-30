<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Аудиоплеер</title>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
        }
        
        body {
            background: linear-gradient(135deg, #0a0a1a 0%, #1a1a2e 100%);
            color: #f0f0f0;
            min-height: 100vh;
            display: flex;
            justify-content: center;
            align-items: center;
            padding: 20px;
        }
        
        .container {
            width: 100%;
            max-width: 480px;
            background: rgba(255, 255, 255, 0.05);
            backdrop-filter: blur(20px);
            border-radius: 24px;
            border: 1px solid rgba(255, 255, 255, 0.1);
            box-shadow: 0 15px 35px rgba(0, 0, 0, 0.5);
            overflow: hidden;
            position: relative;
            padding: 30px;
        }
        
        /* Эффект жидкого стекла */
        .liquid-glass-effect {
            position: absolute;
            width: 200px;
            height: 200px;
            border-radius: 50%;
            background: linear-gradient(45deg, rgba(100, 149, 237, 0.15), rgba(186, 85, 211, 0.1));
            filter: blur(40px);
            opacity: 0.6;
            z-index: -1;
            animation: float 20s infinite ease-in-out;
        }
        
        .liquid-glass-effect:nth-child(1) {
            top: -50px;
            left: -50px;
            animation-delay: 0s;
        }
        
        .liquid-glass-effect:nth-child(2) {
            bottom: -80px;
            right: -50px;
            animation-delay: 5s;
        }
        
        @keyframes float {
            0%, 100% { transform: translate(0, 0) rotate(0deg); }
            33% { transform: translate(30px, -30px) rotate(120deg); }
            66% { transform: translate(-20px, 20px) rotate(240deg); }
        }
        
        .player {
            background: rgba(255, 255, 255, 0.05);
            border-radius: 18px;
            padding: 30px;
            margin-bottom: 20px;
            border: 1px solid rgba(255, 255, 255, 0.08);
        }
        
        .track-info {
            text-align: center;
            margin-bottom: 30px;
        }
        
        .track-title {
            font-size: 22px;
            font-weight: 600;
            margin-bottom: 5px;
            color: #ffffff;
            white-space: nowrap;
            overflow: hidden;
            text-overflow: ellipsis;
        }
        
        .track-artist {
            font-size: 14px;
            color: rgba(255, 255, 255, 0.6);
        }
        
        .progress-area {
            margin-bottom: 30px;
        }
        
        .progress-bar {
            height: 6px;
            width: 100%;
            background: rgba(255, 255, 255, 0.1);
            border-radius: 50px;
            cursor: pointer;
            margin-bottom: 8px;
        }
        
        .progress {
            height: 100%;
            width: 0%;
            background: linear-gradient(90deg, #6495ED, #DA70D6);
            border-radius: inherit;
            position: relative;
            transition: width 0.1s linear;
        }
        
        .progress::after {
            content: '';
            position: absolute;
            height: 14px;
            width: 14px;
            border-radius: 50%;
            background: #ffffff;
            right: -7px;
            top: 50%;
            transform: translateY(-50%);
            box-shadow: 0 0 10px rgba(100, 149, 237, 0.8);
        }
        
        .timer {
            display: flex;
            justify-content: space-between;
            font-size: 13px;
            color: rgba(255, 255, 255, 0.6);
        }
        
        .controls {
            display: flex;
            justify-content: center;
            align-items: center;
            gap: 25px;
            margin-bottom: 10px;
        }
        
        .control-btn {
            background: transparent;
            border: none;
            color: #ffffff;
            font-size: 22px;
            cursor: pointer;
            width: 50px;
            height: 50px;
            border-radius: 50%;
            display: flex;
            justify-content: center;
            align-items: center;
            transition: all 0.3s ease;
        }
        
        .control-btn:hover {
            background: rgba(255, 255, 255, 0.1);
            transform: scale(1.05);
        }
        
        .play-pause {
            width: 60px;
            height: 60px;
            background: linear-gradient(135deg, #6495ED, #DA70D6);
            font-size: 26px;
            box-shadow: 0 5px 15px rgba(100, 149, 237, 0.4);
        }
        
        .play-pause:hover {
            box-shadow: 0 8px 20px rgba(100, 149, 237, 0.6);
            transform: scale(1.08);
        }
        
        .playlist-section {
            margin-top: 20px;
        }
        
        .playlist-title {
            font-size: 18px;
            margin-bottom: 15px;
            color: #ffffff;
            display: flex;
            align-items: center;
            gap: 10px;
        }
        
        .playlist-title i {
            color: #6495ED;
        }
        
        .playlist {
            max-height: 200px;
            overflow-y: auto;
            border-radius: 12px;
            background: rgba(255, 255, 255, 0.03);
            padding: 10px;
        }
        
        .playlist-empty {
            text-align: center;
            padding: 30px 20px;
            color: rgba(255, 255, 255, 0.4);
            font-size: 14px;
        }
        
        .playlist-item {
            padding: 12px 15px;
            border-radius: 10px;
            cursor: pointer;
            transition: all 0.3s ease;
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 8px;
        }
        
        .playlist-item:hover {
            background: rgba(255, 255, 255, 0.08);
        }
        
        .playlist-item.active {
            background: rgba(100, 149, 237, 0.15);
            border-left: 3px solid #6495ED;
        }
        
        .playlist-item-title {
            font-weight: 500;
            font-size: 15px;
            white-space: nowrap;
            overflow: hidden;
            text-overflow: ellipsis;
            flex: 1;
        }
        
        .playlist-item-duration {
            font-size: 13px;
            color: rgba(255, 255, 255, 0.5);
            margin-left: 15px;
        }
        
        .playlist-item-remove {
            color: rgba(255, 255, 255, 0.3);
            font-size: 14px;
            margin-left: 10px;
            padding: 5px;
            border-radius: 4px;
            transition: all 0.3s ease;
        }
        
        .playlist-item-remove:hover {
            color: #ff4757;
            background: rgba(255, 255, 255, 0.05);
        }
        
        .input-section {
            display: flex;
            gap: 10px;
            margin-top: 20px;
        }
        
        .url-input {
            flex: 1;
            background: rgba(255, 255, 255, 0.07);
            border: 1px solid rgba(255, 255, 255, 0.1);
            border-radius: 12px;
            padding: 12px 18px;
            color: #ffffff;
            font-size: 14px;
            outline: none;
            transition: all 0.3s ease;
        }
        
        .url-input:focus {
            border-color: rgba(100, 149, 237, 0.5);
            box-shadow: 0 0 0 2px rgba(100, 149, 237, 0.2);
        }
        
        .add-btn {
            background: linear-gradient(135deg, #6495ED, #DA70D6);
            border: none;
            border-radius: 12px;
            color: white;
            padding: 12px 20px;
            font-weight: 600;
            cursor: pointer;
            transition: all 0.3s ease;
            white-space: nowrap;
        }
        
        .add-btn:hover {
            transform: translateY(-2px);
            box-shadow: 0 5px 15px rgba(100, 149, 237, 0.4);
        }
        
        .status {
            text-align: center;
            margin-top: 15px;
            font-size: 13px;
            color: rgba(255, 255, 255, 0.5);
            min-height: 18px;
        }
        
        /* Полоса прокрутки */
        ::-webkit-scrollbar {
            width: 6px;
        }
        
        ::-webkit-scrollbar-track {
            background: rgba(255, 255, 255, 0.05);
            border-radius: 10px;
        }
        
        ::-webkit-scrollbar-thumb {
            background: rgba(100, 149, 237, 0.5);
            border-radius: 10px;
        }
        
        /* Адаптивность для Telegram Mini App */
        @media (max-height: 700px) {
            .container {
                padding: 20px;
            }
            
            .player {
                padding: 20px;
            }
            
            .playlist {
                max-height: 150px;
            }
        }
    </style>
</head>
<body>
    <!-- Эффекты жидкого стекла -->
    <div class="liquid-glass-effect"></div>
    <div class="liquid-glass-effect"></div>
    
    <div class="container">
        <div class="player">
            <div class="track-info">
                <div class="track-title" id="track-title">Добавьте аудио по ссылке</div>
                <div class="track-artist" id="track-artist">Плейлист пуст</div>
            </div>
            
            <div class="progress-area">
                <div class="progress-bar" id="progress-bar">
                    <div class="progress" id="progress"></div>
                </div>
                <div class="timer">
                    <span id="current-time">0:00</span>
                    <span id="total-time">0:00</span>
                </div>
            </div>
            
            <div class="controls">
                <button class="control-btn" id="prev-btn">
                    <i class="fas fa-step-backward"></i>
                </button>
                
                <button class="control-btn play-pause" id="play-pause-btn">
                    <i class="fas fa-play" id="play-icon"></i>
                </button>
                
                <button class="control-btn" id="next-btn">
                    <i class="fas fa-step-forward"></i>
                </button>
            </div>
        </div>
        
        <div class="playlist-section">
            <div class="playlist-title">
                <i class="fas fa-list-music"></i>
                <span>Плейлист</span>
            </div>
            
            <div class="playlist" id="playlist">
                <div class="playlist-empty" id="empty-playlist">
                    Плейлист пуст<br>
                    <small>Добавьте аудио по ссылке ниже</small>
                </div>
            </div>
        </div>
        
        <div class="input-section">
            <input type="text" class="url-input" id="url-input" placeholder="Вставьте ссылку на аудио (MP3, M3U8, etc.)">
            <button class="add-btn" id="add-btn">Добавить</button>
        </div>
        
        <div class="status" id="status"></div>
    </div>

    <script>
        // Основные элементы DOM
        const audioPlayer = new Audio();
        const playPauseBtn = document.getElementById('play-pause-btn');
        const playIcon = document.getElementById('play-icon');
        const prevBtn = document.getElementById('prev-btn');
        const nextBtn = document.getElementById('next-btn');
        const progressBar = document.getElementById('progress-bar');
        const progress = document.getElementById('progress');
        const currentTimeEl = document.getElementById('current-time');
        const totalTimeEl = document.getElementById('total-time');
        const trackTitle = document.getElementById('track-title');
        const trackArtist = document.getElementById('track-artist');
        const playlistEl = document.getElementById('playlist');
        const emptyPlaylistEl = document.getElementById('empty-playlist');
        const urlInput = document.getElementById('url-input');
        const addBtn = document.getElementById('add-btn');
        const statusEl = document.getElementById('status');
        
        // Пустой плейлист
        let playlist = [];
        let currentTrackIndex = -1;
        let isPlaying = false;
        let isSeeking = false;
        
        // Инициализация плеера
        function initPlayer() {
            // Установка громкости по умолчанию
            audioPlayer.volume = 1.0;
            
            // Обновление статуса
            updateStatus("Готов к воспроизведению");
        }
        
        // Загрузка трека
        function loadTrack(index) {
            if (index < 0 || index >= playlist.length) return;
            
            currentTrackIndex = index;
            const track = playlist[index];
            
            audioPlayer.src = track.url;
            trackTitle.textContent = track.title || "Без названия";
            trackArtist.textContent = track.artist || "Неизвестный исполнитель";
            
            // Обновление активного элемента в плейлисте
            updateActivePlaylistItem();
            
            // Если был воспроизведение, продолжаем
            if (isPlaying) {
                audioPlayer.play();
            }
            
            updateStatus(`Загружен: ${track.title || "Без названия"}`);
        }
        
        // Воспроизведение/пауза
        function togglePlayPause() {
            if (playlist.length === 0) {
                updateStatus("Плейлист пуст. Добавьте треки.");
                return;
            }
            
            if (currentTrackIndex === -1) {
                loadTrack(0);
            }
            
            if (isPlaying) {
                audioPlayer.pause();
                playIcon.classList.remove('fa-pause');
                playIcon.classList.add('fa-play');
                updateStatus("Пауза");
            } else {
                audioPlayer.play();
                playIcon.classList.remove('fa-play');
                playIcon.classList.add('fa-pause');
                updateStatus("Воспроизведение");
            }
            
            isPlaying = !isPlaying;
        }
        
        // Следующий трек
        function playNextTrack() {
            if (playlist.length === 0) return;
            
            let nextIndex = currentTrackIndex + 1;
            if (nextIndex >= playlist.length) {
                nextIndex = 0; // Вернуться к началу
            }
            
            loadTrack(nextIndex);
            
            if (isPlaying) {
                audioPlayer.play();
            }
        }
        
        // Предыдущий трек
        function playPrevTrack() {
            if (playlist.length === 0) return;
            
            let prevIndex = currentTrackIndex - 1;
            if (prevIndex < 0) {
                prevIndex = playlist.length - 1; // Перейти к концу
            }
            
            loadTrack(prevIndex);
            
            if (isPlaying) {
                audioPlayer.play();
            }
        }
        
        // Обновление прогресса
        function updateProgress() {
            if (isSeeking || !audioPlayer.duration) return;
            
            const percent = (audioPlayer.currentTime / audioPlayer.duration) * 100;
            progress.style.width = `${percent}%`;
            
            // Обновление времени
            currentTimeEl.textContent = formatTime(audioPlayer.currentTime);
            totalTimeEl.textContent = formatTime(audioPlayer.duration);
        }
        
        // Перемотка
        function seekTo(event) {
            if (!audioPlayer.duration) return;
            
            const progressBarWidth = progressBar.clientWidth;
            const clickX = event.offsetX;
            const percent = clickX / progressBarWidth;
            
            audioPlayer.currentTime = percent * audioPlayer.duration;
            progress.style.width = `${percent * 100}%`;
        }
        
        // Форматирование времени
        function formatTime(seconds) {
            if (isNaN(seconds)) return "0:00";
            
            const mins = Math.floor(seconds / 60);
            const secs = Math.floor(seconds % 60);
            return `${mins}:${secs < 10 ? '0' : ''}${secs}`;
        }
        
        // Рендер плейлиста
        function renderPlaylist() {
            playlistEl.innerHTML = '';
            
            if (playlist.length === 0) {
                emptyPlaylistEl.style.display = 'block';
                return;
            }
            
            emptyPlaylistEl.style.display = 'none';
            
            playlist.forEach((track, index) => {
                const item = document.createElement('div');
                item.className = `playlist-item ${index === currentTrackIndex ? 'active' : ''}`;
                item.dataset.index = index;
                
                item.innerHTML = `
                    <div class="playlist-item-title">${track.title || "Без названия"}</div>
                    <div class="playlist-item-duration">${track.duration || "?:??"}</div>
                    <div class="playlist-item-remove" title="Удалить">
                        <i class="fas fa-times"></i>
                    </div>
                `;
                
                // Клик по треку
                item.querySelector('.playlist-item-title').addEventListener('click', () => {
                    loadTrack(index);
                    if (isPlaying) {
                        audioPlayer.play();
                    }
                });
                
                // Удаление трека
                const removeBtn = item.querySelector('.playlist-item-remove');
                removeBtn.addEventListener('click', (e) => {
                    e.stopPropagation();
                    removeTrack(index);
                });
                
                playlistEl.appendChild(item);
            });
        }
        
        // Удаление трека из плейлиста
        function removeTrack(index) {
            if (index < 0 || index >= playlist.length) return;
            
            const removedTrack = playlist[index];
            
            // Если удаляем текущий трек
            if (index === currentTrackIndex) {
                if (isPlaying) {
                    audioPlayer.pause();
                    isPlaying = false;
                    playIcon.classList.remove('fa-pause');
                    playIcon.classList.add('fa-play');
                }
                
                audioPlayer.src = '';
                trackTitle.textContent = "Добавьте аудио по ссылке";
                trackArtist.textContent = "Плейлист пуст";
                progress.style.width = '0%';
                currentTimeEl.textContent = "0:00";
                totalTimeEl.textContent = "0:00";
                currentTrackIndex = -1;
            }
            
            // Корректировка текущего индекса
            if (index < currentTrackIndex) {
                currentTrackIndex--;
            }
            
            // Удаление из плейлиста
            playlist.splice(index, 1);
            
            // Рендер обновленного плейлиста
            renderPlaylist();
            
            updateStatus(`Удален: ${removedTrack.title || "Без названия"}`);
        }
        
        // Обновление активного элемента плейлиста
        function updateActivePlaylistItem() {
            const items = document.querySelectorAll('.playlist-item');
            items.forEach((item, index) => {
                if (index === currentTrackIndex) {
                    item.classList.add('active');
                } else {
                    item.classList.remove('active');
                }
            });
        }
        
        // Добавление трека по URL
        function addTrackFromUrl() {
            const url = urlInput.value.trim();
            
            if (!url) {
                updateStatus("Введите URL аудио");
                urlInput.focus();
                return;
            }
            
            // Проверка на валидность URL
            try {
                new URL(url);
            } catch (e) {
                updateStatus("Некорректный URL");
                return;
            }
            
            // Добавление трека в плейлист
            const newTrack = {
                title: `Трек ${playlist.length + 1}`,
                artist: "Неизвестный исполнитель",
                url: url,
                duration: "?:??"
            };
            
            playlist.push(newTrack);
            renderPlaylist();
            
            // Очистка поля ввода
            urlInput.value = '';
            
            updateStatus(`Трек добавлен: ${newTrack.title}`);
            
            // Если это первый трек, автоматически выбираем его
            if (playlist.length === 1) {
                loadTrack(0);
            }
        }
        
        // Обновление статуса
        function updateStatus(message) {
            statusEl.textContent = message;
            
            // Автоматическое скрытие через 3 секунды
            setTimeout(() => {
                if (statusEl.textContent === message) {
                    statusEl.textContent = '';
                }
            }, 3000);
        }
        
        // Интеграция с Telegram Mini App
        function initTelegramIntegration() {
            // Проверяем, запущено ли в Telegram WebView
            if (window.Telegram && Telegram.WebApp) {
                const tg = Telegram.WebApp;
                
                // Инициализация Telegram WebApp
                tg.ready();
                tg.expand();
                
                // Изменение цвета заголовка
                tg.setHeaderColor('#0a0a1a');
                tg.setBackgroundColor('#0a0a1a');
                
                // Добавляем кнопку "Назад" в Telegram
                if (tg.BackButton) {
                    tg.BackButton.show();
                    tg.BackButton.onClick(() => {
                        window.history.back();
                    });
                }
            }
        }
        
        // События плеера
        audioPlayer.addEventListener('timeupdate', updateProgress);
        audioPlayer.addEventListener('loadedmetadata', () => {
            totalTimeEl.textContent = formatTime(audioPlayer.duration);
            
            // Обновляем длительность в плейлисте
            if (playlist[currentTrackIndex]) {
                playlist[currentTrackIndex].duration = formatTime(audioPlayer.duration);
                renderPlaylist();
            }
        });
        audioPlayer.addEventListener('ended', playNextTrack);
        audioPlayer.addEventListener('error', () => {
            updateStatus("Ошибка загрузки аудио");
        });
        
        // События кнопок
        playPauseBtn.addEventListener('click', togglePlayPause);
        prevBtn.addEventListener('click', playPrevTrack);
        nextBtn.addEventListener('click', playNextTrack);
        
        // События прогресса
        progressBar.addEventListener('click', seekTo);
        
        // Добавление трека
        addBtn.addEventListener('click', addTrackFromUrl);
        urlInput.addEventListener('keypress', (e) => {
            if (e.key === 'Enter') {
                addTrackFromUrl();
            }
        });
        
        // Инициализация при загрузке
        window.addEventListener('DOMContentLoaded', () => {
            initPlayer();
            initTelegramIntegration();
        });
    </script>
</body>
</html>
