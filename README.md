<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>Аудиоплеер</title>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <style>
        :root {
            --primary-gradient: linear-gradient(135deg, #6495ED, #DA70D6);
            --glass-bg: rgba(255, 255, 255, 0.05);
            --glass-border: rgba(255, 255, 255, 0.1);
            --text-primary: #ffffff;
            --text-secondary: rgba(255, 255, 255, 0.6);
            --bg-dark: #0a0a1a;
            --bg-darker: #1a1a2e;
            --active-color: rgba(100, 149, 237, 0.15);
        }
        
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            -webkit-tap-highlight-color: transparent;
        }
        
        body {
            font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, sans-serif;
            background: linear-gradient(135deg, var(--bg-dark) 0%, var(--bg-darker) 100%);
            color: var(--text-primary);
            min-height: 100vh;
            display: flex;
            justify-content: center;
            align-items: center;
            padding: 16px;
            overflow-x: hidden;
        }
        
        .container {
            width: 100%;
            max-width: 480px;
            background: var(--glass-bg);
            backdrop-filter: blur(20px);
            -webkit-backdrop-filter: blur(20px);
            border-radius: 24px;
            border: 1px solid var(--glass-border);
            box-shadow: 0 15px 35px rgba(0, 0, 0, 0.5);
            overflow: hidden;
            position: relative;
            padding: 24px;
        }
        
        /* Эффект жидкого стекла - адаптивный */
        .liquid-glass-effect {
            position: absolute;
            width: min(200px, 40vw);
            height: min(200px, 40vw);
            border-radius: 50%;
            background: linear-gradient(45deg, rgba(100, 149, 237, 0.15), rgba(186, 85, 211, 0.1));
            filter: blur(min(40px, 8vw));
            opacity: 0.6;
            z-index: -1;
            animation: float 20s infinite ease-in-out;
        }
        
        .liquid-glass-effect:nth-child(1) {
            top: -10%;
            left: -10%;
            animation-delay: 0s;
        }
        
        .liquid-glass-effect:nth-child(2) {
            bottom: -15%;
            right: -10%;
            animation-delay: 5s;
        }
        
        @keyframes float {
            0%, 100% { transform: translate(0, 0) rotate(0deg); }
            33% { transform: translate(4vw, -4vw) rotate(120deg); }
            66% { transform: translate(-3vw, 3vw) rotate(240deg); }
        }
        
        .player {
            background: rgba(255, 255, 255, 0.03);
            border-radius: 20px;
            padding: clamp(16px, 5vw, 24px);
            margin-bottom: 20px;
            border: 1px solid var(--glass-border);
        }
        
        .track-info {
            text-align: center;
            margin-bottom: clamp(20px, 6vw, 30px);
            padding: 0 8px;
        }
        
        .track-title {
            font-size: clamp(18px, 5vw, 22px);
            font-weight: 600;
            margin-bottom: 4px;
            color: var(--text-primary);
            white-space: nowrap;
            overflow: hidden;
            text-overflow: ellipsis;
            max-width: 100%;
            display: block;
        }
        
        .track-artist {
            font-size: clamp(13px, 3.5vw, 14px);
            color: var(--text-secondary);
        }
        
        .progress-area {
            margin-bottom: clamp(20px, 6vw, 30px);
        }
        
        .progress-bar {
            height: 6px;
            width: 100%;
            background: rgba(255, 255, 255, 0.1);
            border-radius: 50px;
            cursor: pointer;
            margin-bottom: 8px;
            touch-action: none;
        }
        
        .progress {
            height: 100%;
            width: 0%;
            background: var(--primary-gradient);
            border-radius: inherit;
            position: relative;
            transition: width 0.1s linear;
        }
        
        .progress::after {
            content: '';
            position: absolute;
            height: clamp(12px, 3vw, 14px);
            width: clamp(12px, 3vw, 14px);
            border-radius: 50%;
            background: #ffffff;
            right: -6px;
            top: 50%;
            transform: translateY(-50%);
            box-shadow: 0 0 10px rgba(100, 149, 237, 0.8);
        }
        
        .timer {
            display: flex;
            justify-content: space-between;
            font-size: clamp(12px, 3vw, 13px);
            color: var(--text-secondary);
            padding: 0 2px;
        }
        
        .controls {
            display: flex;
            justify-content: center;
            align-items: center;
            gap: clamp(16px, 4vw, 25px);
            margin-bottom: 10px;
        }
        
        .control-btn {
            background: transparent;
            border: none;
            color: var(--text-primary);
            font-size: clamp(20px, 5vw, 22px);
            cursor: pointer;
            width: clamp(44px, 11vw, 50px);
            height: clamp(44px, 11vw, 50px);
            border-radius: 50%;
            display: flex;
            justify-content: center;
            align-items: center;
            transition: all 0.3s ease;
            touch-action: manipulation;
        }
        
        .control-btn:active {
            background: rgba(255, 255, 255, 0.1);
            transform: scale(0.95);
        }
        
        .play-pause {
            width: clamp(52px, 13vw, 60px);
            height: clamp(52px, 13vw, 60px);
            background: var(--primary-gradient);
            font-size: clamp(22px, 5.5vw, 26px);
            box-shadow: 0 5px 15px rgba(100, 149, 237, 0.4);
        }
        
        .play-pause:active {
            box-shadow: 0 8px 20px rgba(100, 149, 237, 0.6);
            transform: scale(0.95);
        }
        
        .playlist-section {
            margin-top: 20px;
        }
        
        .playlist-title {
            font-size: clamp(16px, 4vw, 18px);
            margin-bottom: 12px;
            color: var(--text-primary);
            display: flex;
            align-items: center;
            gap: 8px;
        }
        
        .playlist-title i {
            color: #6495ED;
            font-size: clamp(14px, 3.5vw, 16px);
        }
        
        .playlist {
            max-height: min(200px, 40vh);
            overflow-y: auto;
            border-radius: 12px;
            background: rgba(255, 255, 255, 0.03);
            padding: 10px;
            scrollbar-width: thin;
            scrollbar-color: rgba(100, 149, 237, 0.5) rgba(255, 255, 255, 0.05);
        }
        
        .playlist-empty {
            text-align: center;
            padding: clamp(20px, 6vw, 30px) clamp(16px, 4vw, 20px);
            color: rgba(255, 255, 255, 0.4);
            font-size: clamp(13px, 3.5vw, 14px);
        }
        
        .playlist-empty small {
            display: block;
            margin-top: 8px;
            font-size: clamp(11px, 3vw, 12px);
        }
        
        .playlist-item {
            padding: clamp(10px, 2.5vw, 12px) clamp(12px, 3vw, 15px);
            border-radius: 10px;
            cursor: pointer;
            transition: all 0.3s ease;
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 6px;
            user-select: none;
        }
        
        .playlist-item:active {
            background: rgba(255, 255, 255, 0.08);
        }
        
        .playlist-item.active {
            background: var(--active-color);
            border-left: 3px solid #6495ED;
        }
        
        .playlist-item-title {
            font-weight: 500;
            font-size: clamp(14px, 3.5vw, 15px);
            white-space: nowrap;
            overflow: hidden;
            text-overflow: ellipsis;
            flex: 1;
            min-width: 0;
            padding-right: 8px;
        }
        
        .playlist-item-duration {
            font-size: clamp(12px, 3vw, 13px);
            color: rgba(255, 255, 255, 0.5);
            margin-left: 8px;
            white-space: nowrap;
        }
        
        .playlist-item-remove {
            color: rgba(255, 255, 255, 0.3);
            font-size: clamp(12px, 3vw, 14px);
            margin-left: 8px;
            padding: 4px;
            border-radius: 4px;
            transition: all 0.3s ease;
            flex-shrink: 0;
        }
        
        .playlist-item-remove:active {
            color: #ff4757;
            background: rgba(255, 255, 255, 0.05);
        }
        
        .input-section {
            display: flex;
            gap: clamp(8px, 2vw, 10px);
            margin-top: 20px;
            flex-wrap: wrap;
        }
        
        .url-input {
            flex: 1;
            min-width: 0;
            background: rgba(255, 255, 255, 0.07);
            border: 1px solid var(--glass-border);
            border-radius: 12px;
            padding: clamp(10px, 2.5vw, 12px) clamp(14px, 3vw, 18px);
            color: var(--text-primary);
            font-size: clamp(13px, 3.5vw, 14px);
            outline: none;
            transition: all 0.3s ease;
        }
        
        .url-input:focus {
            border-color: rgba(100, 149, 237, 0.5);
            box-shadow: 0 0 0 2px rgba(100, 149, 237, 0.2);
        }
        
        .add-btn {
            background: var(--primary-gradient);
            border: none;
            border-radius: 12px;
            color: white;
            padding: clamp(10px, 2.5vw, 12px) clamp(16px, 3.5vw, 20px);
            font-weight: 600;
            cursor: pointer;
            transition: all 0.3s ease;
            white-space: nowrap;
            font-size: clamp(13px, 3.5vw, 14px);
            touch-action: manipulation;
        }
        
        .add-btn:active {
            transform: translateY(-1px);
            box-shadow: 0 5px 15px rgba(100, 149, 237, 0.4);
        }
        
        .status {
            text-align: center;
            margin-top: 12px;
            font-size: clamp(12px, 3vw, 13px);
            color: rgba(255, 255, 255, 0.5);
            min-height: 18px;
            padding: 0 8px;
            word-break: break-word;
            line-height: 1.4;
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
        
        /* Пейзажная ориентация */
        @media (orientation: landscape) and (max-height: 600px) {
            body {
                padding: 12px;
                align-items: flex-start;
            }
            
            .container {
                max-width: 90%;
                padding: 16px;
            }
            
            .player {
                padding: 16px;
                margin-bottom: 16px;
            }
            
            .track-info {
                margin-bottom: 16px;
            }
            
            .progress-area {
                margin-bottom: 16px;
            }
            
            .playlist {
                max-height: 150px;
            }
            
            .playlist-empty {
                padding: 20px 16px;
            }
        }
        
        /* Очень маленькие экраны */
        @media (max-width: 360px) {
            .container {
                padding: 16px;
                border-radius: 20px;
            }
            
            .player {
                padding: 14px;
                border-radius: 16px;
            }
            
            .controls {
                gap: 12px;
            }
            
            .input-section {
                flex-direction: column;
            }
            
            .add-btn {
                width: 100%;
                padding: 12px;
            }
        }
        
        /* Планшеты и большие экраны */
        @media (min-width: 768px) {
            body {
                padding: 24px;
            }
            
            .container {
                max-width: 500px;
                padding: 32px;
            }
            
            .player {
                padding: 28px;
            }
            
            .track-info {
                margin-bottom: 32px;
            }
            
            .progress-area {
                margin-bottom: 32px;
            }
            
            .controls {
                gap: 28px;
            }
        }
        
        /* Темная тема по умолчанию */
        @media (prefers-color-scheme: light) {
            :root {
                --glass-bg: rgba(0, 0, 0, 0.05);
                --glass-border: rgba(0, 0, 0, 0.1);
                --text-primary: #333333;
                --text-secondary: rgba(0, 0, 0, 0.6);
                --bg-dark: #f5f5f7;
                --bg-darker: #e5e5ea;
                --active-color: rgba(100, 149, 237, 0.1);
            }
            
            .progress::after {
                background: #333333;
            }
            
            .playlist-item-remove {
                color: rgba(0, 0, 0, 0.3);
            }
            
            .playlist-item-remove:active {
                background: rgba(0, 0, 0, 0.05);
            }
        }
        
        /* Анимация загрузки */
        @keyframes pulse {
            0%, 100% { opacity: 1; }
            50% { opacity: 0.5; }
        }
        
        .loading {
            animation: pulse 1.5s ease-in-out infinite;
        }
        
        /* Для устройств с курсором */
        @media (hover: hover) and (pointer: fine) {
            .control-btn:hover {
                background: rgba(255, 255, 255, 0.1);
                transform: scale(1.05);
            }
            
            .play-pause:hover {
                box-shadow: 0 8px 20px rgba(100, 149, 237, 0.6);
                transform: scale(1.08);
            }
            
            .playlist-item:hover {
                background: rgba(255, 255, 255, 0.08);
            }
            
            .playlist-item-remove:hover {
                color: #ff4757;
                background: rgba(255, 255, 255, 0.05);
            }
            
            .add-btn:hover {
                transform: translateY(-2px);
                box-shadow: 0 5px 15px rgba(100, 149, 237, 0.4);
            }
        }
        
        /* Адаптация для телефонов с вырезом (notch) */
        @supports (padding-top: env(safe-area-inset-top)) {
            body {
                padding-top: env(safe-area-inset-top);
                padding-bottom: env(safe-area-inset-bottom);
                padding-left: env(safe-area-inset-left);
                padding-right: env(safe-area-inset-right);
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
                <button class="control-btn" id="prev-btn" aria-label="Предыдущий трек">
                    <i class="fas fa-step-backward"></i>
                </button>
                
                <button class="control-btn play-pause" id="play-pause-btn" aria-label="Воспроизвести/пауза">
                    <i class="fas fa-play" id="play-icon"></i>
                </button>
                
                <button class="control-btn" id="next-btn" aria-label="Следующий трек">
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
            <input type="text" class="url-input" id="url-input" 
                   placeholder="Вставьте ссылку на аудио" 
                   aria-label="Ссылка на аудиофайл">
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
        let touchStartX = 0;
        
        // Инициализация плеера
        function initPlayer() {
            // Установка громкости по умолчанию
            audioPlayer.volume = 1.0;
            
            // Обновление статуса
            updateStatus("Готов к воспроизведению");
            
            // Восстановление плейлиста из localStorage
            loadPlaylistFromStorage();
        }
        
        // Сохранение плейлиста в localStorage
        function savePlaylistToStorage() {
            try {
                localStorage.setItem('audioPlayerPlaylist', JSON.stringify(playlist));
                localStorage.setItem('audioPlayerCurrentIndex', currentTrackIndex.toString());
            } catch (e) {
                console.log('Не удалось сохранить плейлист');
            }
        }
        
        // Загрузка плейлиста из localStorage
        function loadPlaylistFromStorage() {
            try {
                const savedPlaylist = localStorage.getItem('audioPlayerPlaylist');
                const savedIndex = localStorage.getItem('audioPlayerCurrentIndex');
                
                if (savedPlaylist) {
                    playlist = JSON.parse(savedPlaylist);
                    currentTrackIndex = savedIndex ? parseInt(savedIndex) : -1;
                    
                    if (playlist.length > 0) {
                        renderPlaylist();
                        
                        if (currentTrackIndex >= 0 && currentTrackIndex < playlist.length) {
                            loadTrack(currentTrackIndex, false);
                        }
                    }
                }
            } catch (e) {
                console.log('Не удалось загрузить плейлист');
            }
        }
        
        // Загрузка трека
        function loadTrack(index, autoPlay = true) {
            if (index < 0 || index >= playlist.length) return;
            
            currentTrackIndex = index;
            const track = playlist[index];
            
            // Показываем индикатор загрузки
            trackTitle.classList.add('loading');
            trackArtist.textContent = 'Загрузка...';
            
            audioPlayer.src = track.url;
            
            audioPlayer.onloadeddata = () => {
                trackTitle.textContent = track.title || "Без названия";
                trackArtist.textContent = track.artist || "Неизвестный исполнитель";
                trackTitle.classList.remove('loading');
                
                // Сохраняем текущий индекс
                savePlaylistToStorage();
            };
            
            audioPlayer.onerror = () => {
                trackTitle.textContent = track.title || "Без названия";
                trackArtist.textContent = "Ошибка загрузки";
                trackTitle.classList.remove('loading');
                updateStatus("Ошибка загрузки аудио");
            };
            
            // Обновление активного элемента в плейлисте
            updateActivePlaylistItem();
            
            // Если был воспроизведение, продолжаем
            if (isPlaying && autoPlay) {
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
                audioPlayer.play().catch(e => {
                    updateStatus("Ошибка воспроизведения");
                    console.error('Playback error:', e);
                });
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
        
        // Перемотка (мышь и тач)
        function startSeek(event) {
            if (!audioPlayer.duration) return;
            
            isSeeking = true;
            const clientX = event.type.includes('touch') ? event.touches[0].clientX : event.clientX;
            const progressBarRect = progressBar.getBoundingClientRect();
            const clickX = clientX - progressBarRect.left;
            seekToPosition(clickX, progressBarRect.width);
        }
        
        function seekToPosition(clickX, barWidth) {
            const percent = Math.max(0, Math.min(1, clickX / barWidth));
            audioPlayer.currentTime = percent * audioPlayer.duration;
            progress.style.width = `${percent * 100}%`;
        }
        
        function stopSeek() {
            isSeeking = false;
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
                playlistEl.appendChild(emptyPlaylistEl);
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
                    <div class="playlist-item-remove" title="Удалить" aria-label="Удалить трек">
                        <i class="fas fa-times"></i>
                    </div>
                `;
                
                // Клик по треку
                item.addEventListener('click', (e) => {
                    if (!e.target.closest('.playlist-item-remove')) {
                        loadTrack(index);
                        if (isPlaying) {
                            audioPlayer.play();
                        }
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
            
            // Сохранение в хранилище
            savePlaylistToStorage();
            
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
        async function addTrackFromUrl() {
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
                urlInput.focus();
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
            
            // Сохранение в хранилище
            savePlaylistToStorage();
            
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
        
        // Адаптация для разных устройств
        function initResponsiveFeatures() {
            // Обработка сенсорных событий для прогресс-бара
            progressBar.addEventListener('touchstart', (e) => {
                touchStartX = e.touches[0].clientX;
                startSeek(e);
            }, { passive: true });
            
            progressBar.addEventListener('touchmove', (e) => {
                if (!isSeeking) return;
                e.preventDefault();
                const clientX = e.touches[0].clientX;
                const progressBarRect = progressBar.getBoundingClientRect();
                const clickX = clientX - progressBarRect.left;
                seekToPosition(clickX, progressBarRect.width);
            });
            
            progressBar.addEventListener('touchend', stopSeek);
            
            // Обработка мыши для прогресс-бара
            progressBar.addEventListener('mousedown', startSeek);
            document.addEventListener('mousemove', (e) => {
                if (!isSeeking) return;
                const progressBarRect = progressBar.getBoundingClientRect();
                const clickX = e.clientX - progressBarRect.left;
                seekToPosition(clickX, progressBarRect.width);
            });
            document.addEventListener('mouseup', stopSeek);
            
            // Предотвращение масштабирования при двойном тапе на кнопках
            const buttons = document.querySelectorAll('button');
            buttons.forEach(btn => {
                btn.addEventListener('touchstart', (e) => {
                    if (e.touches.length > 1) e.preventDefault();
                }, { passive: false });
            });
            
            // Адаптация к изменению ориентации
            window.addEventListener('orientationchange', () => {
                setTimeout(() => {
                    window.scrollTo(0, 0);
                }, 100);
            });
        }
        
        // События плеера
        audioPlayer.addEventListener('timeupdate', updateProgress);
        audioPlayer.addEventListener('loadedmetadata', () => {
            totalTimeEl.textContent = formatTime(audioPlayer.duration);
            
            // Обновляем длительность в плейлисте
            if (playlist[currentTrackIndex]) {
                playlist[currentTrackIndex].duration = formatTime(audioPlayer.duration);
                renderPlaylist();
                savePlaylistToStorage();
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
            initResponsiveFeatures();
        });
        
        // Сохранение состояния при закрытии
        window.addEventListener('beforeunload', () => {
            savePlaylistToStorage();
        });
    </script>
</body>
</html>
