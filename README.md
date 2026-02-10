<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>Аудиоплеер</title>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            -webkit-tap-highlight-color: transparent;
            -webkit-font-smoothing: antialiased;
        }

        body {
            font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, sans-serif;
            background: #0f0f23;
            color: #ffffff;
            min-height: 100vh;
            padding: 12px;
            display: flex;
            justify-content: center;
            align-items: center;
            line-height: 1.4;
        }

        .container {
            width: 100%;
            max-width: 500px;
            background: #1a1a2e;
            border-radius: 16px;
            overflow: hidden;
            box-shadow: 0 4px 20px rgba(0, 0, 0, 0.3);
        }

        /* Player section */
        .player {
            background: #16213e;
            padding: 20px;
            border-bottom: 1px solid rgba(255, 255, 255, 0.1);
        }

        .now-playing {
            display: flex;
            align-items: center;
            margin-bottom: 24px;
        }

        .cover {
            width: 60px;
            height: 60px;
            background: linear-gradient(135deg, #667eea, #764ba2);
            border-radius: 10px;
            display: flex;
            justify-content: center;
            align-items: center;
            font-size: 1.5rem;
            margin-right: 16px;
            flex-shrink: 0;
        }

        .track-info h2 {
            font-size: 1.1rem;
            font-weight: 600;
            margin-bottom: 4px;
            white-space: nowrap;
            overflow: hidden;
            text-overflow: ellipsis;
            max-width: 250px;
        }

        .track-info p {
            color: #8a8a9e;
            font-size: 0.85rem;
        }

        .controls {
            display: flex;
            justify-content: center;
            align-items: center;
            gap: 20px;
        }

        .control-btn {
            background: rgba(255, 255, 255, 0.1);
            border: none;
            color: #ffffff;
            font-size: 1.1rem;
            cursor: pointer;
            width: 44px;
            height: 44px;
            border-radius: 50%;
            display: flex;
            justify-content: center;
            align-items: center;
            transition: all 0.2s ease;
            touch-action: manipulation;
        }

        .control-btn:active {
            transform: scale(0.95);
            background: rgba(255, 255, 255, 0.2);
        }

        .play-btn {
            background: linear-gradient(135deg, #667eea, #764ba2);
            width: 50px;
            height: 50px;
            font-size: 1.3rem;
        }

        .play-btn:active {
            transform: scale(0.95);
        }

        /* Input area */
        .input-area {
            background: #1a1a2e;
            padding: 16px;
            border-bottom: 1px solid rgba(255, 255, 255, 0.1);
        }

        .input-group {
            display: flex;
            gap: 8px;
        }

        .input-group input {
            flex: 1;
            padding: 12px 14px;
            border-radius: 10px;
            border: 1px solid rgba(255, 255, 255, 0.1);
            background: rgba(255, 255, 255, 0.05);
            color: #ffffff;
            font-size: 0.95rem;
            outline: none;
        }

        .input-group input:focus {
            border-color: #667eea;
        }

        .input-group input::placeholder {
            color: #8a8a9e;
        }

        .input-group button {
            padding: 12px 16px;
            border-radius: 10px;
            border: none;
            background: linear-gradient(135deg, #667eea, #764ba2);
            color: white;
            font-weight: 600;
            font-size: 0.95rem;
            cursor: pointer;
            transition: all 0.2s ease;
            touch-action: manipulation;
            white-space: nowrap;
        }

        .input-group button:active {
            transform: scale(0.98);
        }

        /* Playlist */
        .playlist {
            background: #1a1a2e;
            padding: 16px;
            max-height: 300px;
            overflow-y: auto;
            -webkit-overflow-scrolling: touch;
        }

        .playlist::-webkit-scrollbar {
            width: 4px;
        }

        .playlist::-webkit-scrollbar-track {
            background: rgba(255, 255, 255, 0.05);
        }

        .playlist::-webkit-scrollbar-thumb {
            background: rgba(255, 255, 255, 0.2);
            border-radius: 2px;
        }

        .playlist ul {
            list-style: none;
        }

        .playlist li {
            padding: 12px;
            border-radius: 10px;
            margin-bottom: 8px;
            cursor: pointer;
            display: flex;
            align-items: center;
            justify-content: space-between;
            background: rgba(255, 255, 255, 0.03);
            transition: all 0.2s ease;
            touch-action: manipulation;
        }

        .playlist li:active {
            background: rgba(255, 255, 255, 0.08);
            transform: scale(0.99);
        }

        .playlist li.active {
            background: rgba(102, 126, 234, 0.15);
            border-left: 3px solid #667eea;
        }

        .track-content {
            flex: 1;
            overflow: hidden;
            margin-right: 10px;
        }

        .track-title {
            font-weight: 500;
            margin-bottom: 2px;
            white-space: nowrap;
            overflow: hidden;
            text-overflow: ellipsis;
            max-width: 180px;
            font-size: 0.95rem;
        }

        .track-title.editing {
            background: rgba(255, 255, 255, 0.1);
            border: 1px solid #667eea;
            cursor: text;
            white-space: normal;
        }

        .track-title-input {
            background: transparent;
            border: 1px solid #667eea;
            color: #fff;
            font-size: 0.95rem;
            font-weight: 500;
            padding: 4px 6px;
            border-radius: 6px;
            width: 100%;
            font-family: inherit;
            outline: none;
        }

        .track-url {
            color: #8a8a9e;
            font-size: 0.8rem;
            white-space: nowrap;
            overflow: hidden;
            text-overflow: ellipsis;
            max-width: 180px;
        }

        .track-actions {
            display: flex;
            gap: 6px;
            flex-shrink: 0;
        }

        .edit-btn, .delete-btn {
            background: rgba(255, 255, 255, 0.1);
            border: none;
            color: #fff;
            width: 32px;
            height: 32px;
            border-radius: 50%;
            display: flex;
            justify-content: center;
            align-items: center;
            cursor: pointer;
            transition: all 0.2s ease;
            touch-action: manipulation;
            font-size: 0.9rem;
        }

        .edit-btn:active, .delete-btn:active {
            transform: scale(0.9);
        }

        .edit-btn:hover, .edit-btn:active {
            background: rgba(102, 126, 234, 0.3);
            color: #a3b4ff;
        }

        .delete-btn:hover, .delete-btn:active {
            background: rgba(255, 107, 107, 0.3);
            color: #ff9e9e;
        }

        .empty-playlist {
            text-align: center;
            color: #8a8a9e;
            padding: 24px 16px;
            font-style: italic;
            font-size: 0.95rem;
        }

        /* Адаптивность для маленьких экранов */
        @media (max-width: 480px) {
            body {
                padding: 8px;
            }
            
            .container {
                border-radius: 12px;
            }
            
            .player {
                padding: 16px;
            }
            
            .cover {
                width: 50px;
                height: 50px;
                font-size: 1.3rem;
            }
            
            .track-info h2 {
                font-size: 1rem;
                max-width: 200px;
            }
            
            .track-info p {
                font-size: 0.8rem;
            }
            
            .controls {
                gap: 16px;
            }
            
            .control-btn {
                width: 40px;
                height: 40px;
                font-size: 1rem;
            }
            
            .play-btn {
                width: 46px;
                height: 46px;
                font-size: 1.2rem;
            }
            
            .input-area {
                padding: 14px;
            }
            
            .input-group input {
                padding: 10px 12px;
                font-size: 0.9rem;
            }
            
            .input-group button {
                padding: 10px 14px;
                font-size: 0.9rem;
            }
            
            .playlist {
                padding: 14px;
                max-height: 250px;
            }
            
            .playlist li {
                padding: 10px;
            }
            
            .track-title {
                max-width: 140px;
                font-size: 0.9rem;
            }
            
            .track-url {
                max-width: 140px;
                font-size: 0.75rem;
            }
            
            .edit-btn, .delete-btn {
                width: 28px;
                height: 28px;
                font-size: 0.8rem;
            }
        }

        @media (max-width: 360px) {
            .cover {
                width: 44px;
                height: 44px;
                margin-right: 12px;
            }
            
            .track-info h2 {
                max-width: 160px;
            }
            
            .track-title {
                max-width: 120px;
            }
            
            .track-url {
                max-width: 120px;
            }
            
            .input-group {
                flex-direction: column;
            }
            
            .input-group button {
                width: 100%;
            }
        }

        /* Для очень высоких экранов */
        @media (min-height: 700px) {
            .playlist {
                max-height: 350px;
            }
        }

        @media (min-height: 800px) {
            .playlist {
                max-height: 400px;
            }
        }

        /* Анимации для Telegram */
        .fade-in {
            animation: fadeIn 0.3s ease;
        }

        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(10px); }
            to { opacity: 1; transform: translateY(0); }
        }

        /* Улучшение доступности для тач-устройств */
        @media (hover: none) and (pointer: coarse) {
            .control-btn:hover,
            .input-group button:hover,
            .playlist li:hover,
            .edit-btn:hover,
            .delete-btn:hover {
                transform: none;
            }
            
            .control-btn:active,
            .input-group button:active,
            .playlist li:active,
            .edit-btn:active,
            .delete-btn:active {
                transform: scale(0.95);
            }
        }
    </style>
</head>
<body>
    <div class="container">
        <div class="player">
            <div class="now-playing">
                <div class="cover">
                    <i class="fas fa-music"></i>
                </div>
                <div class="track-info">
                    <h2 id="current-title">Добавьте аудио</h2>
                    <p id="current-url">Плейлист пуст</p>
                </div>
            </div>
            
            <div class="controls">
                <button class="control-btn" id="prev-btn">
                    <i class="fas fa-step-backward"></i>
                </button>
                <button class="control-btn play-btn" id="play-btn">
                    <i class="fas fa-play"></i>
                </button>
                <button class="control-btn" id="next-btn">
                    <i class="fas fa-step-forward"></i>
                </button>
            </div>
        </div>
        
        <div class="input-area">
            <div class="input-group">
                <input type="url" id="audio-url" placeholder="Вставьте ссылку на аудио" inputmode="url">
                <button id="add-btn">Добавить</button>
            </div>
        </div>
        
        <div class="playlist">
            <ul id="playlist">
                <div class="empty-playlist" id="empty-msg">
                    Плейлист пуст. Добавьте аудио.
                </div>
            </ul>
        </div>
    </div>

    <script>
        document.addEventListener('DOMContentLoaded', function() {
            // Элементы
            const audio = new Audio();
            const playBtn = document.getElementById('play-btn');
            const prevBtn = document.getElementById('prev-btn');
            const nextBtn = document.getElementById('next-btn');
            const currentTitle = document.getElementById('current-title');
            const currentUrl = document.getElementById('current-url');
            const playlistEl = document.getElementById('playlist');
            const emptyMsg = document.getElementById('empty-msg');
            const urlInput = document.getElementById('audio-url');
            const addBtn = document.getElementById('add-btn');
            
            // Состояние
            let playing = false;
            let currentIndex = -1;
            let editingIndex = -1;
            let playlist = [];
            
            // Инициализация
            function init() {
                updatePlaylist();
                audio.volume = 1.0;
                
                // События
                playBtn.addEventListener('click', togglePlay);
                prevBtn.addEventListener('click', prev);
                nextBtn.addEventListener('click', next);
                addBtn.addEventListener('click', addTrack);
                urlInput.addEventListener('keypress', e => {
                    if (e.key === 'Enter') addTrack();
                });
                
                audio.addEventListener('ended', next);
                audio.addEventListener('play', () => {
                    playing = true;
                    updatePlayButton();
                });
                audio.addEventListener('pause', () => {
                    playing = false;
                    updatePlayButton();
                });
                
                audio.addEventListener('error', function() {
                    currentTitle.textContent = "Ошибка";
                    currentUrl.textContent = "Проверьте ссылку";
                    setTimeout(() => {
                        if (playlist.length > 0) next();
                    }, 2000);
                });
                
                updateButtons();
                
                // Фокус на поле ввода при загрузке
                setTimeout(() => urlInput.focus(), 300);
            }
            
            // Загрузка трека
            function loadTrack(index) {
                if (playlist.length === 0 || index < 0 || index >= playlist.length) {
                    resetPlayer();
                    return;
                }
                
                currentIndex = index;
                const track = playlist[currentIndex];
                
                audio.src = track.url;
                currentTitle.textContent = track.title;
                currentUrl.textContent = track.url.length > 40 ? track.url.substring(0, 40) + '...' : track.url;
                
                // Обновление активного элемента
                document.querySelectorAll('.playlist li').forEach((item, i) => {
                    item.classList.toggle('active', i === currentIndex);
                });
                
                if (playing) {
                    audio.play().catch(e => {
                        console.log("Ошибка:", e);
                        playing = false;
                        updatePlayButton();
                    });
                }
            }
            
            // Сброс
            function resetPlayer() {
                audio.src = '';
                currentTitle.textContent = 'Добавьте аудио';
                currentUrl.textContent = 'Плейлист пуст';
                playing = false;
                updatePlayButton();
                document.querySelectorAll('.playlist li').forEach(item => {
                    item.classList.remove('active');
                });
            }
            
            // Воспроизведение/пауза
            function togglePlay() {
                if (playlist.length === 0) {
                    alert('Добавьте аудио в плейлист');
                    return;
                }
                
                if (currentIndex === -1) {
                    currentIndex = 0;
                    loadTrack(currentIndex);
                }
                
                if (playing) {
                    audio.pause();
                } else {
                    audio.play().catch(e => {
                        console.log("Ошибка:", e);
                        next();
                    });
                }
            }
            
            // Обновление кнопки воспроизведения
            function updatePlayButton() {
                const icon = playBtn.querySelector('i');
                icon.className = playing ? 'fas fa-pause' : 'fas fa-play';
            }
            
            // Предыдущий трек
            function prev() {
                if (playlist.length === 0) return;
                
                currentIndex = currentIndex <= 0 ? playlist.length - 1 : currentIndex - 1;
                loadTrack(currentIndex);
                if (playing) audio.play();
            }
            
            // Следующий трек
            function next() {
                if (playlist.length === 0) return;
                
                currentIndex = currentIndex >= playlist.length - 1 ? 0 : currentIndex + 1;
                loadTrack(currentIndex);
                if (playing) audio.play();
            }
            
            // Редактирование названия
            function editTitle(index) {
                if (editingIndex !== -1 && editingIndex !== index) {
                    saveTitle(editingIndex);
                }
                
                editingIndex = index;
                const items = document.querySelectorAll('.playlist li');
                const titleEl = items[index].querySelector('.track-title');
                
                const input = document.createElement('input');
                input.className = 'track-title-input';
                input.value = playlist[index].title;
                
                titleEl.style.display = 'none';
                titleEl.parentNode.insertBefore(input, titleEl);
                input.focus();
                input.select();
                
                input.addEventListener('keypress', e => {
                    if (e.key === 'Enter') saveTitle(index);
                });
                
                input.addEventListener('blur', () => {
                    setTimeout(() => saveTitle(index), 100);
                });
                
                titleEl.classList.add('editing');
                if (index === currentIndex) {
                    currentTitle.textContent = playlist[index].title;
                }
            }
            
            // Сохранение названия
            function saveTitle(index) {
                if (editingIndex === -1) return;
                
                const items = document.querySelectorAll('.playlist li');
                const input = items[index].querySelector('.track-title-input');
                if (!input) return;
                
                const newTitle = input.value.trim();
                const titleEl = items[index].querySelector('.track-title');
                
                input.parentNode.removeChild(input);
                titleEl.style.display = 'block';
                titleEl.classList.remove('editing');
                
                if (newTitle) {
                    playlist[index].title = newTitle;
                    titleEl.textContent = newTitle;
                    if (index === currentIndex) currentTitle.textContent = newTitle;
                }
                
                editingIndex = -1;
            }
            
            // Добавление трека
            function addTrack() {
                const url = urlInput.value.trim();
                if (!url) {
                    alert('Введите URL');
                    return;
                }
                
                try {
                    new URL(url);
                } catch {
                    alert('Некорректный URL');
                    return;
                }
                
                if (playlist.some(track => track.url === url)) {
                    alert('Трек уже есть');
                    return;
                }
                
                let title = `Трек ${playlist.length + 1}`;
                try {
                    const urlObj = new URL(url);
                    const path = urlObj.pathname.split('/');
                    const file = path[path.length - 1];
                    if (file && file.includes('.')) {
                        title = file.split('.')[0];
                    }
                } catch(e) {}
                
                playlist.push({ title, url });
                updatePlaylist();
                
                if (playlist.length === 1) {
                    currentIndex = 0;
                    loadTrack(currentIndex);
                }
                
                updateButtons();
                urlInput.value = '';
                urlInput.focus();
            }
            
            // Удаление трека
            function removeTrack(index, e) {
                e.stopPropagation();
                
                if (editingIndex === index) {
                    saveTitle(index);
                }
                
                if (index === currentIndex) {
                    if (playing) audio.pause();
                    playlist.splice(index, 1);
                    
                    if (playlist.length === 0) {
                        resetPlayer();
                        currentIndex = -1;
                    } else {
                        if (index >= playlist.length) currentIndex = playlist.length - 1;
                        loadTrack(currentIndex);
                        if (playing) audio.play();
                    }
                } else {
                    playlist.splice(index, 1);
                    if (index < currentIndex) currentIndex--;
                }
                
                updatePlaylist();
                updateButtons();
            }
            
            // Обновление плейлиста
            function updatePlaylist() {
                playlistEl.innerHTML = '';
                
                if (playlist.length === 0) {
                    emptyMsg.style.display = 'block';
                    playlistEl.appendChild(emptyMsg);
                    return;
                }
                
                emptyMsg.style.display = 'none';
                
                playlist.forEach((track, index) => {
                    const li = document.createElement('li');
                    li.className = 'fade-in';
                    
                    const content = document.createElement('div');
                    content.className = 'track-content';
                    
                    const title = document.createElement('div');
                    title.className = 'track-title';
                    title.textContent = track.title;
                    title.title = 'Кликните для редактирования';
                    
                    const url = document.createElement('div');
                    url.className = 'track-url';
                    url.textContent = track.url.length > 40 ? track.url.substring(0, 40) + '...' : track.url;
                    url.title = track.url;
                    
                    content.appendChild(title);
                    content.appendChild(url);
                    
                    const actions = document.createElement('div');
                    actions.className = 'track-actions';
                    
                    const editBtn = document.createElement('button');
                    editBtn.className = 'edit-btn';
                    editBtn.innerHTML = '<i class="fas fa-edit"></i>';
                    editBtn.title = 'Редактировать';
                    editBtn.addEventListener('click', (e) => {
                        e.stopPropagation();
                        editTitle(index);
                    });
                    
                    const deleteBtn = document.createElement('button');
                    deleteBtn.className = 'delete-btn';
                    deleteBtn.innerHTML = '<i class="fas fa-times"></i>';
                    deleteBtn.title = 'Удалить';
                    deleteBtn.addEventListener('click', (e) => removeTrack(index, e));
                    
                    actions.appendChild(editBtn);
                    actions.appendChild(deleteBtn);
                    
                    li.appendChild(content);
                    li.appendChild(actions);
                    
                    // Редактирование по двойному клику
                    title.addEventListener('dblclick', (e) => {
                        e.stopPropagation();
                        editTitle(index);
                    });
                    
                    // Редактирование по долгому нажатию (для мобильных)
                    let pressTimer;
                    title.addEventListener('touchstart', (e) => {
                        e.stopPropagation();
                        pressTimer = setTimeout(() => {
                            editTitle(index);
                        }, 500);
                    });
                    
                    title.addEventListener('touchend', () => {
                        clearTimeout(pressTimer);
                    });
                    
                    title.addEventListener('touchmove', () => {
                        clearTimeout(pressTimer);
                    });
                    
                    li.addEventListener('click', () => {
                        loadTrack(index);
                        if (!playing) togglePlay();
                    });
                    
                    playlistEl.appendChild(li);
                });
                
                // Активный элемент
                const items = document.querySelectorAll('.playlist li');
                if (items.length > 0 && currentIndex >= 0 && currentIndex < items.length) {
                    items[currentIndex].classList.add('active');
                }
            }
            
            // Обновление кнопок
            function updateButtons() {
                const hasTracks = playlist.length > 0;
                const canNav = playlist.length > 1;
                
                playBtn.disabled = !hasTracks;
                prevBtn.disabled = !canNav;
                nextBtn.disabled = !canNav;
                
                playBtn.style.opacity = hasTracks ? '1' : '0.5';
                prevBtn.style.opacity = canNav ? '1' : '0.5';
                nextBtn.style.opacity = canNav ? '1' : '0.5';
                
                playBtn.style.cursor = hasTracks ? 'pointer' : 'not-allowed';
                prevBtn.style.cursor = canNav ? 'pointer' : 'not-allowed';
                nextBtn.style.cursor = canNav ? 'pointer' : 'not-allowed';
            }
            
            // Запуск
            init();
        });
    </script>
</body>
</html>
