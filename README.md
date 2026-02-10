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
            font-family: 'Segoe UI', Tahoma, sans-serif;
        }

        body {
            background: linear-gradient(135deg, #1a1a2e 0%, #16213e 100%);
            color: #fff;
            min-height: 100vh;
            padding: 20px;
            display: flex;
            justify-content: center;
            align-items: center;
        }

        .container {
            width: 100%;
            max-width: 800px;
            background: rgba(255, 255, 255, 0.08);
            backdrop-filter: blur(10px);
            border-radius: 20px;
            overflow: hidden;
            box-shadow: 0 10px 30px rgba(0, 0, 0, 0.3);
        }

        .player {
            background: rgba(0, 0, 0, 0.2);
            border-radius: 15px;
            padding: 30px;
            margin: 30px;
        }

        .now-playing {
            display: flex;
            align-items: center;
            margin-bottom: 40px;
        }

        .cover {
            width: 80px;
            height: 80px;
            background: linear-gradient(45deg, #00b4db, #0083b0);
            border-radius: 10px;
            display: flex;
            justify-content: center;
            align-items: center;
            font-size: 2rem;
            margin-right: 20px;
            flex-shrink: 0;
        }

        .track-info h2 {
            font-size: 1.3rem;
            margin-bottom: 5px;
            white-space: nowrap;
            overflow: hidden;
            text-overflow: ellipsis;
            max-width: 300px;
        }

        .track-info p {
            color: #aaa;
            font-size: 0.9rem;
        }

        .controls {
            display: flex;
            justify-content: center;
            align-items: center;
            gap: 20px;
        }

        .control-btn {
            background: transparent;
            border: none;
            color: #fff;
            font-size: 1.3rem;
            cursor: pointer;
            width: 45px;
            height: 45px;
            border-radius: 50%;
            display: flex;
            justify-content: center;
            align-items: center;
            transition: background 0.3s;
        }

        .control-btn:hover {
            background: rgba(255, 255, 255, 0.1);
        }

        .play-btn {
            background: linear-gradient(135deg, #00b4db, #0083b0);
            width: 55px;
            height: 55px;
            font-size: 1.5rem;
        }

        .play-btn:hover {
            transform: scale(1.05);
        }

        .input-area {
            background: rgba(0, 0, 0, 0.2);
            border-radius: 15px;
            padding: 20px;
            margin: 0 30px 20px;
        }

        .input-group {
            display: flex;
            gap: 10px;
        }

        .input-group input {
            flex: 1;
            padding: 10px 15px;
            border-radius: 10px;
            border: none;
            background: rgba(255, 255, 255, 0.1);
            color: #fff;
            font-size: 0.95rem;
        }

        .input-group input::placeholder {
            color: #aaa;
        }

        .input-group button {
            padding: 10px 15px;
            border-radius: 10px;
            border: none;
            background: linear-gradient(90deg, #00b4db, #0083b0);
            color: white;
            font-weight: 600;
            cursor: pointer;
            transition: transform 0.3s;
        }

        .input-group button:hover {
            transform: scale(1.05);
        }

        .playlist {
            background: rgba(0, 0, 0, 0.2);
            border-radius: 15px;
            padding: 20px;
            margin: 0 30px 30px;
            max-height: 250px;
            overflow-y: auto;
        }

        .playlist ul {
            list-style: none;
        }

        .playlist li {
            padding: 10px 12px;
            border-radius: 8px;
            margin-bottom: 8px;
            cursor: pointer;
            display: flex;
            align-items: center;
            justify-content: space-between;
            background: rgba(255, 255, 255, 0.05);
            transition: background 0.3s;
        }

        .playlist li:hover {
            background: rgba(255, 255, 255, 0.1);
        }

        .playlist li.active {
            background: rgba(0, 180, 219, 0.2);
            border-left: 3px solid #00b4db;
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
            max-width: 200px;
            cursor: pointer;
            padding: 2px 4px;
            border-radius: 4px;
            transition: background 0.2s;
        }

        .track-title:hover {
            background: rgba(255, 255, 255, 0.05);
        }

        .track-title.editing {
            background: rgba(255, 255, 255, 0.1);
            border: 1px solid #00b4db;
            cursor: text;
            white-space: normal;
        }

        .track-title-input {
            background: transparent;
            border: 1px solid #00b4db;
            color: #fff;
            font-size: 0.95rem;
            font-weight: 500;
            padding: 2px 4px;
            border-radius: 4px;
            width: 100%;
            font-family: inherit;
        }

        .track-url {
            color: #aaa;
            font-size: 0.8rem;
            white-space: nowrap;
            overflow: hidden;
            text-overflow: ellipsis;
            max-width: 200px;
        }

        .track-actions {
            display: flex;
            gap: 5px;
            flex-shrink: 0;
        }

        .edit-btn, .delete-btn {
            background: rgba(255, 255, 255, 0.1);
            border: none;
            color: #fff;
            width: 28px;
            height: 28px;
            border-radius: 50%;
            display: flex;
            justify-content: center;
            align-items: center;
            cursor: pointer;
            transition: background 0.3s;
        }

        .edit-btn:hover {
            background: rgba(0, 180, 219, 0.3);
            color: #00b4db;
        }

        .delete-btn:hover {
            background: rgba(255, 0, 0, 0.3);
            color: #ff6b6b;
        }

        .empty-playlist {
            text-align: center;
            color: #aaa;
            padding: 15px;
            font-style: italic;
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
                <input type="url" id="audio-url" placeholder="URL аудио">
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
                playBtn.querySelector('i').className = playing ? 'fas fa-pause' : 'fas fa-play';
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
                    
                    title.addEventListener('dblclick', (e) => {
                        e.stopPropagation();
                        editTitle(index);
                    });
                    
                    title.addEventListener('click', (e) => {
                        if (editingIndex !== index) {
                            e.stopPropagation();
                            editTitle(index);
                        }
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
