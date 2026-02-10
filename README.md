<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Потоковый аудиоплеер</title>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
        }

        body {
            background: linear-gradient(135deg, #1a1a2e 0%, #16213e 100%);
            color: #f1f1f1;
            min-height: 100vh;
            padding: 20px;
            display: flex;
            justify-content: center;
            align-items: center;
        }

        .container {
            max-width: 800px;
            width: 100%;
            background-color: rgba(255, 255, 255, 0.08);
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
            width: 100px;
            height: 100px;
            background: linear-gradient(45deg, #00b4db, #0083b0);
            border-radius: 10px;
            display: flex;
            justify-content: center;
            align-items: center;
            font-size: 2.5rem;
            margin-right: 20px;
            flex-shrink: 0;
        }

        .track-info h2 {
            font-size: 1.5rem;
            margin-bottom: 5px;
            white-space: nowrap;
            overflow: hidden;
            text-overflow: ellipsis;
            max-width: 400px;
        }

        .track-info p {
            color: #aaa;
            font-size: 1rem;
        }

        .controls {
            display: flex;
            justify-content: center;
            align-items: center;
            gap: 25px;
        }

        .control-btn {
            background: transparent;
            border: none;
            color: #fff;
            font-size: 1.5rem;
            cursor: pointer;
            transition: all 0.3s;
            width: 50px;
            height: 50px;
            border-radius: 50%;
            display: flex;
            justify-content: center;
            align-items: center;
        }

        .control-btn:hover {
            background: rgba(255, 255, 255, 0.1);
        }

        .play-btn {
            background: linear-gradient(135deg, #00b4db, #0083b0);
            width: 60px;
            height: 60px;
            font-size: 1.8rem;
        }

        .play-btn:hover {
            transform: scale(1.05);
        }

        .link-input-area {
            background: rgba(0, 0, 0, 0.2);
            border-radius: 15px;
            padding: 25px;
            margin: 0 30px 30px;
        }

        .input-group {
            display: flex;
            gap: 10px;
        }

        .input-group input {
            flex: 1;
            padding: 12px 15px;
            border-radius: 10px;
            border: none;
            background: rgba(255, 255, 255, 0.1);
            color: #fff;
            font-size: 1rem;
        }

        .input-group input::placeholder {
            color: #aaa;
        }

        .input-group button {
            padding: 12px 20px;
            border-radius: 10px;
            border: none;
            background: linear-gradient(90deg, #00b4db, #0083b0);
            color: white;
            font-weight: 600;
            cursor: pointer;
            transition: all 0.3s;
        }

        .input-group button:hover {
            transform: scale(1.05);
        }

        .playlist-section {
            background: rgba(0, 0, 0, 0.2);
            border-radius: 15px;
            padding: 25px;
            margin: 0 30px 30px;
            max-height: 300px;
            overflow-y: auto;
        }

        .playlist {
            list-style: none;
        }

        .playlist li {
            padding: 12px 15px;
            border-radius: 10px;
            margin-bottom: 10px;
            cursor: pointer;
            transition: all 0.3s;
            display: flex;
            align-items: center;
            justify-content: space-between;
            background: rgba(255, 255, 255, 0.05);
        }

        .playlist li:hover {
            background: rgba(255, 255, 255, 0.1);
        }

        .playlist li.active {
            background: rgba(0, 180, 219, 0.2);
            border-left: 4px solid #00b4db;
        }

        .track-info-container {
            flex: 1;
            overflow: hidden;
            margin-right: 10px;
        }

        .track-title {
            font-weight: 500;
            margin-bottom: 3px;
            white-space: nowrap;
            overflow: hidden;
            text-overflow: ellipsis;
            max-width: 250px;
            cursor: pointer;
            transition: all 0.2s;
            padding: 2px 5px;
            border-radius: 4px;
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
            font-size: 1rem;
            font-weight: 500;
            padding: 2px 5px;
            border-radius: 4px;
            width: 100%;
            font-family: inherit;
        }

        .track-url {
            color: #aaa;
            font-size: 0.85rem;
            white-space: nowrap;
            overflow: hidden;
            text-overflow: ellipsis;
            max-width: 250px;
        }

        .track-actions {
            display: flex;
            gap: 5px;
            flex-shrink: 0;
        }

        .edit-btn, .remove-btn {
            background: rgba(255, 255, 255, 0.1);
            border: none;
            color: #fff;
            width: 30px;
            height: 30px;
            border-radius: 50%;
            display: flex;
            justify-content: center;
            align-items: center;
            cursor: pointer;
            transition: all 0.3s;
        }

        .edit-btn:hover {
            background: rgba(0, 180, 219, 0.3);
            color: #00b4db;
        }

        .remove-btn:hover {
            background: rgba(255, 0, 0, 0.3);
            color: #ff6b6b;
        }

        .empty-playlist {
            text-align: center;
            color: #aaa;
            padding: 20px;
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
                    <h2 id="current-track-title">Добавьте аудио по ссылке</h2>
                    <p id="current-track-artist">Плейлист пуст</p>
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
        
        <div class="link-input-area">
            <div class="input-group">
                <input type="url" id="audio-url" placeholder="Введите URL аудиофайла или потокового радио">
                <button id="add-url-btn">Добавить</button>
            </div>
        </div>
        
        <div class="playlist-section">
            <ul class="playlist" id="playlist">
                <div class="empty-playlist" id="empty-playlist">
                    Плейлист пуст. Добавьте первую аудиозапись по ссылке выше.
                </div>
            </ul>
        </div>
    </div>

    <script>
        document.addEventListener('DOMContentLoaded', function() {
            // Элементы плеера
            const audioPlayer = new Audio();
            const playBtn = document.getElementById('play-btn');
            const prevBtn = document.getElementById('prev-btn');
            const nextBtn = document.getElementById('next-btn');
            const currentTrackTitle = document.getElementById('current-track-title');
            const currentTrackArtist = document.getElementById('current-track-artist');
            const playlistEl = document.getElementById('playlist');
            const emptyPlaylistMsg = document.getElementById('empty-playlist');
            const audioUrlInput = document.getElementById('audio-url');
            const addUrlBtn = document.getElementById('add-url-btn');
            
            // Состояние плеера
            let isPlaying = false;
            let currentTrackIndex = -1; // -1 означает, что ничего не играет
            let editingTrackIndex = -1; // -1 означает, что ни один трек не редактируется
            
            // Плейлист (изначально пустой)
            let playlist = [];
            
            // Инициализация плеера
            function initPlayer() {
                updatePlaylistDisplay();
                
                // Устанавливаем стандартную громкость на максимум
                audioPlayer.volume = 1.0;
                
                // События плеера
                playBtn.addEventListener('click', togglePlay);
                prevBtn.addEventListener('click', prevTrack);
                nextBtn.addEventListener('click', nextTrack);
                addUrlBtn.addEventListener('click', addUrlToPlaylist);
                
                // Нажатие Enter в поле ввода
                audioUrlInput.addEventListener('keypress', function(e) {
                    if (e.key === 'Enter') {
                        addUrlToPlaylist();
                    }
                });
                
                // События аудио
                audioPlayer.addEventListener('ended', nextTrack);
                audioPlayer.addEventListener('play', () => {
                    isPlaying = true;
                    updatePlayButton();
                });
                audioPlayer.addEventListener('pause', () => {
                    isPlaying = false;
                    updatePlayButton();
                });
                
                // События ошибок
                audioPlayer.addEventListener('error', function() {
                    console.log("Ошибка загрузки аудио");
                    currentTrackTitle.textContent = "Ошибка загрузки";
                    currentTrackArtist.textContent = "Проверьте ссылку на аудио";
                    
                    // Пробуем следующий трек
                    setTimeout(() => {
                        if (playlist.length > 0) {
                            nextTrack();
                        }
                    }, 2000);
                });
                
                // Блокируем кнопки, пока плейлист пуст
                updateControlsState();
            }
            
            // Загрузка трека
            function loadTrack(index) {
                if (playlist.length === 0 || index < 0 || index >= playlist.length) {
                    resetPlayer();
                    return;
                }
                
                currentTrackIndex = index;
                const track = playlist[currentTrackIndex];
                
                audioPlayer.src = track.url;
                currentTrackTitle.textContent = track.title;
                currentTrackArtist.textContent = track.url.length > 50 ? track.url.substring(0, 50) + '...' : track.url;
                
                // Обновляем активный элемент плейлиста
                const playlistItems = document.querySelectorAll('.playlist li');
                playlistItems.forEach((item, i) => {
                    item.classList.toggle('active', i === currentTrackIndex);
                });
                
                // Автовоспроизведение при загрузке
                if (isPlaying) {
                    audioPlayer.play().catch(e => {
                        console.log("Ошибка воспроизведения:", e);
                        isPlaying = false;
                        updatePlayButton();
                    });
                }
            }
            
            // Сброс плеера
            function resetPlayer() {
                audioPlayer.src = '';
                currentTrackTitle.textContent = 'Добавьте аудио по ссылке';
                currentTrackArtist.textContent = 'Плейлист пуст';
                isPlaying = false;
                updatePlayButton();
                
                const playlistItems = document.querySelectorAll('.playlist li');
                playlistItems.forEach(item => {
                    item.classList.remove('active');
                });
            }
            
            // Воспроизведение/пауза
            function togglePlay() {
                if (playlist.length === 0) {
                    alert('Добавьте хотя бы одну аудиозапись в плейлист');
                    return;
                }
                
                // Если ничего не играет, начинаем с первого трека
                if (currentTrackIndex === -1) {
                    currentTrackIndex = 0;
                    loadTrack(currentTrackIndex);
                }
                
                if (isPlaying) {
                    audioPlayer.pause();
                } else {
                    audioPlayer.play().catch(e => {
                        console.log("Ошибка воспроизведения:", e);
                        // Если ошибка, пробуем следующую дорожку
                        nextTrack();
                    });
                }
            }
            
            // Обновление кнопки воспроизведения
            function updatePlayButton() {
                const icon = playBtn.querySelector('i');
                icon.className = isPlaying ? 'fas fa-pause' : 'fas fa-play';
            }
            
            // Предыдущий трек
            function prevTrack() {
                if (playlist.length === 0) return;
                
                if (currentTrackIndex <= 0) {
                    currentTrackIndex = playlist.length - 1;
                } else {
                    currentTrackIndex--;
                }
                
                loadTrack(currentTrackIndex);
                if (isPlaying) {
                    audioPlayer.play();
                }
            }
            
            // Следующий трек
            function nextTrack() {
                if (playlist.length === 0) return;
                
                if (currentTrackIndex >= playlist.length - 1) {
                    currentTrackIndex = 0;
                } else {
                    currentTrackIndex++;
                }
                
                loadTrack(currentTrackIndex);
                if (isPlaying) {
                    audioPlayer.play();
                }
            }
            
            // Редактирование названия трека
            function editTrackTitle(index) {
                // Если уже редактируем другой трек, сохраняем его
                if (editingTrackIndex !== -1 && editingTrackIndex !== index) {
                    saveTrackTitle(editingTrackIndex);
                }
                
                editingTrackIndex = index;
                const track = playlist[index];
                
                // Находим элемент названия трека
                const playlistItems = document.querySelectorAll('.playlist li');
                const trackTitleElement = playlistItems[index].querySelector('.track-title');
                const trackUrlElement = playlistItems[index].querySelector('.track-url');
                
                // Создаем поле ввода
                const input = document.createElement('input');
                input.type = 'text';
                input.className = 'track-title-input';
                input.value = track.title;
                
                // Заменяем текст на поле ввода
                trackTitleElement.style.display = 'none';
                trackTitleElement.parentNode.insertBefore(input, trackTitleElement);
                input.focus();
                input.select();
                
                // Сохраняем при нажатии Enter
                input.addEventListener('keypress', function(e) {
                    if (e.key === 'Enter') {
                        saveTrackTitle(index);
                    }
                });
                
                // Сохраняем при потере фокуса
                input.addEventListener('blur', function() {
                    setTimeout(() => {
                        saveTrackTitle(index);
                    }, 100);
                });
                
                // Обновляем внешний вид элемента
                trackTitleElement.classList.add('editing');
                
                // Если это текущий трек, обновляем его название в плеере
                if (index === currentTrackIndex) {
                    currentTrackTitle.textContent = track.title;
                }
            }
            
            // Сохранение названия трека
            function saveTrackTitle(index) {
                if (editingTrackIndex === -1) return;
                
                const playlistItems = document.querySelectorAll('.playlist li');
                const input = playlistItems[index].querySelector('.track-title-input');
                
                if (!input) return;
                
                const newTitle = input.value.trim();
                const trackTitleElement = playlistItems[index].querySelector('.track-title');
                
                // Удаляем поле ввода
                input.parentNode.removeChild(input);
                trackTitleElement.style.display = 'block';
                trackTitleElement.classList.remove('editing');
                
                // Если название не пустое, сохраняем
                if (newTitle) {
                    playlist[index].title = newTitle;
                    trackTitleElement.textContent = newTitle;
                    
                    // Если это текущий трек, обновляем его название в плеере
                    if (index === currentTrackIndex) {
                        currentTrackTitle.textContent = newTitle;
                    }
                }
                
                editingTrackIndex = -1;
            }
            
            // Добавление URL в плейлист
            function addUrlToPlaylist() {
                const url = audioUrlInput.value.trim();
                
                if (!url) {
                    alert('Введите URL аудиофайла');
                    return;
                }
                
                // Проверяем валидность URL
                try {
                    new URL(url);
                } catch {
                    alert('Введите корректный URL');
                    return;
                }
                
                // Проверяем, есть ли уже такой URL в плейлисте
                const exists = playlist.some(track => track.url === url);
                if (exists) {
                    alert('Этот трек уже есть в плейлисте');
                    return;
                }
                
                // Извлекаем название из URL или используем номер трека
                let title = `Трек ${playlist.length + 1}`;
                try {
                    const urlObj = new URL(url);
                    const pathParts = urlObj.pathname.split('/');
                    const fileName = pathParts[pathParts.length - 1];
                    if (fileName && fileName.includes('.')) {
                        title = fileName.split('.')[0];
                    }
                } catch(e) {
                    // Если не удалось извлечь название, оставляем стандартное
                }
                
                // Добавляем новый трек
                const newTrack = {
                    title: title,
                    url: url
                };
                
                playlist.push(newTrack);
                updatePlaylistDisplay();
                
                // Если это первый трек, загружаем его
                if (playlist.length === 1) {
                    currentTrackIndex = 0;
                    loadTrack(currentTrackIndex);
                }
                
                // Обновляем состояние кнопок
                updateControlsState();
                
                // Очищаем поле ввода
                audioUrlInput.value = '';
                audioUrlInput.focus();
            }
            
            // Удаление трека из плейлиста
            function removeTrack(index, e) {
                e.stopPropagation(); // Предотвращаем срабатывание клика по элементу плейлиста
                
                if (editingTrackIndex === index) {
                    saveTrackTitle(index);
                }
                
                if (index === currentTrackIndex) {
                    // Если удаляем текущий трек
                    if (isPlaying) {
                        audioPlayer.pause();
                    }
                    
                    playlist.splice(index, 1);
                    
                    if (playlist.length === 0) {
                        resetPlayer();
                        currentTrackIndex = -1;
                    } else {
                        // Если удалили не последний трек, загружаем следующий или предыдущий
                        if (index >= playlist.length) {
                            currentTrackIndex = playlist.length - 1;
                        }
                        loadTrack(currentTrackIndex);
                        if (isPlaying) {
                            audioPlayer.play();
                        }
                    }
                } else {
                    // Если удаляем не текущий трек
                    playlist.splice(index, 1);
                    
                    // Корректируем currentTrackIndex если нужно
                    if (index < currentTrackIndex) {
                        currentTrackIndex--;
                    }
                }
                
                updatePlaylistDisplay();
                updateControlsState();
            }
            
            // Очистка всего плейлиста
            function clearPlaylist() {
                if (playlist.length === 0) return;
                
                if (confirm('Вы уверены, что хотите очистить весь плейлист?')) {
                    playlist = [];
                    currentTrackIndex = -1;
                    editingTrackIndex = -1;
                    resetPlayer();
                    updatePlaylistDisplay();
                    updateControlsState();
                }
            }
            
            // Обновление отображения плейлиста
            function updatePlaylistDisplay() {
                playlistEl.innerHTML = '';
                
                if (playlist.length === 0) {
                    emptyPlaylistMsg.style.display = 'block';
                    playlistEl.appendChild(emptyPlaylistMsg);
                    return;
                }
                
                emptyPlaylistMsg.style.display = 'none';
                
                playlist.forEach((track, index) => {
                    const li = document.createElement('li');
                    
                    const trackInfoContainer = document.createElement('div');
                    trackInfoContainer.className = 'track-info-container';
                    
                    const title = document.createElement('div');
                    title.className = 'track-title';
                    title.textContent = track.title;
                    title.title = 'Кликните для редактирования названия';
                    
                    const url = document.createElement('div');
                    url.className = 'track-url';
                    url.textContent = track.url.length > 40 ? track.url.substring(0, 40) + '...' : track.url;
                    url.title = track.url;
                    
                    trackInfoContainer.appendChild(title);
                    trackInfoContainer.appendChild(url);
                    
                    const trackActions = document.createElement('div');
                    trackActions.className = 'track-actions';
                    
                    const editBtn = document.createElement('button');
                    editBtn.className = 'edit-btn';
                    editBtn.innerHTML = '<i class="fas fa-edit"></i>';
                    editBtn.title = 'Редактировать название';
                    editBtn.addEventListener('click', (e) => {
                        e.stopPropagation();
                        editTrackTitle(index);
                    });
                    
                    const removeBtn = document.createElement('button');
                    removeBtn.className = 'remove-btn';
                    removeBtn.innerHTML = '<i class="fas fa-times"></i>';
                    removeBtn.title = 'Удалить трек';
                    removeBtn.addEventListener('click', (e) => removeTrack(index, e));
                    
                    trackActions.appendChild(editBtn);
                    trackActions.appendChild(removeBtn);
                    
                    li.appendChild(trackInfoContainer);
                    li.appendChild(trackActions);
                    
                    // Редактирование по двойному клику на название
                    title.addEventListener('dblclick', (e) => {
                        e.stopPropagation();
                        editTrackTitle(index);
                    });
                    
                    // Редактирование по клику (если уже не редактируется)
                    title.addEventListener('click', (e) => {
                        if (editingTrackIndex !== index) {
                            e.stopPropagation();
                            editTrackTitle(index);
                        }
                    });
                    
                    li.addEventListener('click', () => {
                        loadTrack(index);
                        if (!isPlaying) {
                            togglePlay();
                        }
                    });
                    
                    playlistEl.appendChild(li);
                });
                
                // Обновляем активный элемент
                const playlistItems = document.querySelectorAll('.playlist li');
                if (playlistItems.length > 0 && currentTrackIndex >= 0 && currentTrackIndex < playlistItems.length) {
                    playlistItems[currentTrackIndex].classList.add('active');
                }
            }
            
            // Обновление состояния кнопок управления
            function updateControlsState() {
                const hasTracks = playlist.length > 0;
                const canPrevNext = playlist.length > 1;
                
                playBtn.disabled = !hasTracks;
                prevBtn.disabled = !canPrevNext;
                nextBtn.disabled = !canPrevNext;
                
                // Визуальное отключение кнопок
                playBtn.style.opacity = hasTracks ? '1' : '0.5';
                prevBtn.style.opacity = canPrevNext ? '1' : '0.5';
                nextBtn.style.opacity = canPrevNext ? '1' : '0.5';
                
                playBtn.style.cursor = hasTracks ? 'pointer' : 'not-allowed';
                prevBtn.style.cursor = canPrevNext ? 'pointer' : 'not-allowed';
                nextBtn.style.cursor = canPrevNext ? 'pointer' : 'not-allowed';
            }
            
            // Запускаем плеер
            initPlayer();
        });
    </script>
</body>
</html>
