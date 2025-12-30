<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Glass Audio</title>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'SF Pro Display', -apple-system, sans-serif;
            -webkit-tap-highlight-color: transparent;
        }

        body {
            background: linear-gradient(135deg, 
                rgba(20, 20, 60, 0.95) 0%, 
                rgba(10, 10, 40, 0.95) 50%, 
                rgba(5, 5, 30, 0.95) 100%);
            color: rgba(255, 255, 255, 0.95);
            min-height: 100vh;
            padding: 0;
            overflow-x: hidden;
            position: relative;
        }

        body::before {
            content: '';
            position: fixed;
            top: 0;
            left: 0;
            right: 0;
            bottom: 0;
            background: 
                radial-gradient(circle at 20% 50%, rgba(120, 119, 198, 0.15) 0%, transparent 50%),
                radial-gradient(circle at 80% 20%, rgba(255, 119, 230, 0.1) 0%, transparent 50%),
                radial-gradient(circle at 40% 80%, rgba(100, 220, 255, 0.1) 0%, transparent 50%);
            z-index: -1;
            animation: liquidMove 20s ease-in-out infinite alternate;
        }

        @keyframes liquidMove {
            0% { transform: translate(0, 0) scale(1); }
            100% { transform: translate(-50px, -30px) scale(1.1); }
        }

        .container {
            max-width: 100%;
            margin: 0 auto;
            padding: 20px;
            backdrop-filter: blur(20px);
            min-height: 100vh;
        }

        .now-playing-card {
            background: linear-gradient(135deg, 
                rgba(255, 255, 255, 0.12) 0%, 
                rgba(255, 255, 255, 0.08) 100%);
            border-radius: 28px;
            padding: 28px;
            margin-bottom: 28px;
            box-shadow: 
                0 8px 32px rgba(0, 0, 0, 0.2),
                inset 0 1px 0 rgba(255, 255, 255, 0.1);
            text-align: center;
            position: relative;
            overflow: hidden;
            border: 1px solid rgba(255, 255, 255, 0.1);
        }

        .now-playing-card::before {
            content: '';
            position: absolute;
            top: 0;
            left: 0;
            right: 0;
            height: 1px;
            background: linear-gradient(90deg, transparent, rgba(255, 255, 255, 0.3), transparent);
        }

        .track-title {
            font-size: 1.8rem;
            font-weight: 600;
            color: rgba(255, 255, 255, 0.98);
            overflow: hidden;
            text-overflow: ellipsis;
            white-space: nowrap;
            margin-bottom: 16px;
            text-shadow: 0 2px 10px rgba(0, 0, 0, 0.3);
        }

        .progress-area {
            margin: 28px 0;
            position: relative;
        }

        .progress-bar {
            height: 6px;
            width: 100%;
            background: rgba(255, 255, 255, 0.1);
            border-radius: 3px;
            cursor: pointer;
            margin-bottom: 10px;
            position: relative;
            overflow: hidden;
        }

        .progress {
            height: 100%;
            width: 0%;
            background: linear-gradient(90deg, 
                rgba(120, 119, 198, 0.9), 
                rgba(255, 119, 230, 0.9));
            border-radius: 3px;
            transition: width 0.1s linear;
            position: relative;
            box-shadow: 0 0 20px rgba(120, 119, 198, 0.3);
        }

        .progress::after {
            content: '';
            position: absolute;
            top: 0;
            left: 0;
            right: 0;
            bottom: 0;
            background: linear-gradient(90deg, transparent, rgba(255, 255, 255, 0.2), transparent);
            animation: progressShimmer 1.5s infinite linear;
        }

        @keyframes progressShimmer {
            0% { transform: translateX(-100%); }
            100% { transform: translateX(100%); }
        }

        .time-info {
            display: flex;
            justify-content: space-between;
            font-size: 0.85rem;
            color: rgba(255, 255, 255, 0.7);
            font-weight: 500;
        }

        .controls {
            display: flex;
            justify-content: center;
            align-items: center;
            gap: 20px;
            margin: 28px 0;
        }

        .control-btn {
            background: linear-gradient(135deg, 
                rgba(255, 255, 255, 0.15) 0%, 
                rgba(255, 255, 255, 0.05) 100%);
            border: 1px solid rgba(255, 255, 255, 0.1);
            width: 56px;
            height: 56px;
            border-radius: 50%;
            color: rgba(255, 255, 255, 0.95);
            font-size: 1.3rem;
            cursor: pointer;
            transition: all 0.3s ease;
            display: flex;
            align-items: center;
            justify-content: center;
            backdrop-filter: blur(10px);
            box-shadow: 0 4px 20px rgba(0, 0, 0, 0.2);
        }

        .control-btn:active {
            transform: scale(0.92);
            background: linear-gradient(135deg, 
                rgba(255, 255, 255, 0.2) 0%, 
                rgba(255, 255, 255, 0.1) 100%);
        }

        .play-btn {
            width: 72px;
            height: 72px;
            font-size: 1.8rem;
            background: linear-gradient(135deg, 
                rgba(120, 119, 198, 0.9) 0%, 
                rgba(255, 119, 230, 0.9) 100%);
            border: 1px solid rgba(255, 255, 255, 0.3);
            box-shadow: 0 8px 32px rgba(120, 119, 198, 0.4);
        }

        .play-btn:active {
            transform: scale(0.95);
        }

        .playlist-section {
            margin-top: 36px;
        }

        .section-header {
            display: flex;
            justify-content: flex-end;
            align-items: center;
            margin-bottom: 24px;
            padding: 0 8px;
        }

        .section-controls {
            display: flex;
            gap: 12px;
        }

        .add-track-btn, .edit-playlist-btn {
            background: linear-gradient(135deg, 
                rgba(120, 119, 198, 0.9) 0%, 
                rgba(255, 119, 230, 0.9) 100%);
            color: rgba(255, 255, 255, 0.98);
            border: 1px solid rgba(255, 255, 255, 0.2);
            padding: 12px 20px;
            border-radius: 16px;
            font-size: 0.95rem;
            font-weight: 600;
            cursor: pointer;
            display: flex;
            align-items: center;
            gap: 8px;
            transition: all 0.3s ease;
            backdrop-filter: blur(10px);
            box-shadow: 0 4px 20px rgba(120, 119, 198, 0.3);
        }

        .edit-playlist-btn {
            background: linear-gradient(135deg, 
                rgba(255, 255, 255, 0.15) 0%, 
                rgba(255, 255, 255, 0.05) 100%);
            box-shadow: 0 4px 20px rgba(0, 0, 0, 0.2);
        }

        .edit-playlist-btn.active {
            background: linear-gradient(135deg, 
                rgba(120, 119, 198, 0.9) 0%, 
                rgba(255, 119, 230, 0.9) 100%);
            box-shadow: 0 4px 20px rgba(120, 119, 198, 0.3);
        }

        .add-track-btn:active, .edit-playlist-btn:active {
            transform: scale(0.95);
        }

        .playlist {
            list-style: none;
        }

        .playlist-item {
            padding: 20px;
            background: linear-gradient(135deg, 
                rgba(255, 255, 255, 0.08) 0%, 
                rgba(255, 255, 255, 0.04) 100%);
            border-radius: 20px;
            margin-bottom: 12px;
            display: flex;
            align-items: center;
            cursor: pointer;
            transition: all 0.3s ease;
            border: 1px solid rgba(255, 255, 255, 0.05);
            backdrop-filter: blur(10px);
            box-shadow: 0 4px 20px rgba(0, 0, 0, 0.15);
            position: relative;
            overflow: hidden;
        }

        .playlist-item::before {
            content: '';
            position: absolute;
            top: 0;
            left: 0;
            right: 0;
            height: 1px;
            background: linear-gradient(90deg, transparent, rgba(255, 255, 255, 0.1), transparent);
        }

        .playlist-item:active {
            transform: scale(0.98);
            background: linear-gradient(135deg, 
                rgba(255, 255, 255, 0.12) 0%, 
                rgba(255, 255, 255, 0.08) 100%);
        }

        .playlist-item.active {
            background: linear-gradient(135deg, 
                rgba(120, 119, 198, 0.2) 0%, 
                rgba(255, 119, 230, 0.2) 100%);
            border: 1px solid rgba(120, 119, 198, 0.3);
            box-shadow: 0 4px 25px rgba(120, 119, 198, 0.25);
        }

        .playlist-item.active::after {
            content: '';
            position: absolute;
            left: 0;
            top: 0;
            bottom: 0;
            width: 4px;
            background: linear-gradient(180deg, 
                rgba(120, 119, 198, 0.9), 
                rgba(255, 119, 230, 0.9));
            border-radius: 0 2px 2px 0;
        }

        .playlist-icon {
            font-size: 1.3rem;
            margin-right: 20px;
            color: rgba(120, 119, 198, 0.9);
            width: 28px;
            text-align: center;
        }

        .playlist-info {
            flex: 1;
            min-width: 0;
        }

        .playlist-title {
            font-weight: 500;
            overflow: hidden;
            text-overflow: ellipsis;
            white-space: nowrap;
            color: rgba(255, 255, 255, 0.95);
            font-size: 1.05rem;
        }

        .playlist-controls {
            display: flex;
            align-items: center;
            gap: 12px;
        }

        .edit-btn, .delete-btn, .drag-handle {
            background: none;
            border: 1px solid rgba(255, 255, 255, 0.1);
            color: rgba(255, 255, 255, 0.7);
            font-size: 1.1rem;
            cursor: pointer;
            padding: 8px;
            border-radius: 10px;
            transition: all 0.3s ease;
            display: flex;
            align-items: center;
            justify-content: center;
            min-width: 40px;
            min-height: 40px;
        }

        .delete-btn {
            color: rgba(255, 107, 107, 0.9);
            border-color: rgba(255, 107, 107, 0.2);
        }

        .edit-btn {
            color: rgba(120, 119, 198, 0.9);
            border-color: rgba(120, 119, 198, 0.2);
        }

        .drag-handle {
            color: rgba(255, 255, 255, 0.5);
            cursor: grab;
        }

        .drag-handle:active {
            cursor: grabbing;
        }

        .edit-btn:active, .delete-btn:active, .drag-handle:active {
            transform: scale(0.95);
        }

        .empty-playlist {
            text-align: center;
            padding: 80px 20px;
            color: rgba(255, 255, 255, 0.5);
            background: linear-gradient(135deg, 
                rgba(255, 255, 255, 0.05) 0%, 
                rgba(255, 255, 255, 0.02) 100%);
            border-radius: 20px;
            border: 1px solid rgba(255, 255, 255, 0.05);
            backdrop-filter: blur(10px);
            box-shadow: 0 4px 20px rgba(0, 0, 0, 0.1);
        }

        .empty-playlist i {
            font-size: 3.5rem;
            margin-bottom: 20px;
            opacity: 0.3;
        }

        .empty-playlist p {
            margin-bottom: 12px;
            font-size: 1.1rem;
        }

        .modal {
            position: fixed;
            top: 0;
            left: 0;
            right: 0;
            bottom: 0;
            background: rgba(0, 0, 0, 0.7);
            backdrop-filter: blur(10px);
            display: flex;
            justify-content: center;
            align-items: center;
            z-index: 1000;
            padding: 20px;
            opacity: 0;
            visibility: hidden;
            transition: all 0.3s ease;
        }

        .modal.active {
            opacity: 1;
            visibility: visible;
        }

        .modal-content {
            background: linear-gradient(135deg, 
                rgba(255, 255, 255, 0.12) 0%, 
                rgba(255, 255, 255, 0.08) 100%);
            border-radius: 28px;
            padding: 32px;
            width: 100%;
            max-width: 420px;
            max-height: 90vh;
            overflow-y: auto;
            transform: translateY(30px) scale(0.95);
            transition: transform 0.4s ease;
            border: 1px solid rgba(255, 255, 255, 0.1);
            box-shadow: 0 20px 60px rgba(0, 0, 0, 0.3);
            position: relative;
            overflow: hidden;
        }

        .modal-content::before {
            content: '';
            position: absolute;
            top: 0;
            left: 0;
            right: 0;
            height: 1px;
            background: linear-gradient(90deg, transparent, rgba(255, 255, 255, 0.3), transparent);
        }

        .modal.active .modal-content {
            transform: translateY(0) scale(1);
        }

        .modal-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 28px;
            padding-bottom: 20px;
            border-bottom: 1px solid rgba(255, 255, 255, 0.1);
        }

        .modal-title {
            font-size: 1.5rem;
            font-weight: 700;
            color: rgba(255, 255, 255, 0.98);
            text-shadow: 0 2px 8px rgba(0, 0, 0, 0.3);
        }

        .close-btn {
            background: rgba(255, 255, 255, 0.1);
            border: 1px solid rgba(255, 255, 255, 0.1);
            color: rgba(255, 255, 255, 0.7);
            font-size: 1.3rem;
            cursor: pointer;
            padding: 10px;
            border-radius: 12px;
            transition: all 0.3s ease;
            display: flex;
            align-items: center;
            justify-content: center;
            width: 44px;
            height: 44px;
        }

        .close-btn:hover {
            background: rgba(255, 255, 255, 0.15);
            transform: rotate(90deg);
        }

        .close-btn:active {
            transform: scale(0.9) rotate(90deg);
        }

        .form-group {
            margin-bottom: 24px;
        }

        .form-label {
            display: block;
            margin-bottom: 12px;
            font-size: 0.95rem;
            color: rgba(255, 255, 255, 0.8);
            font-weight: 500;
        }

        .form-input {
            width: 100%;
            padding: 16px 20px;
            background: rgba(255, 255, 255, 0.07);
            border: 1px solid rgba(255, 255, 255, 0.1);
            border-radius: 16px;
            color: rgba(255, 255, 255, 0.95);
            font-size: 1rem;
            transition: all 0.3s ease;
            backdrop-filter: blur(10px);
            box-shadow: inset 0 2px 4px rgba(0, 0, 0, 0.1);
        }

        .form-input:focus {
            outline: none;
            border-color: rgba(120, 119, 198, 0.5);
            background: rgba(255, 255, 255, 0.1);
        }

        .form-input::placeholder {
            color: rgba(255, 255, 255, 0.4);
        }

        .submit-btn {
            width: 100%;
            padding: 18px;
            background: linear-gradient(135deg, 
                rgba(120, 119, 198, 0.9) 0%, 
                rgba(255, 119, 230, 0.9) 100%);
            border: 1px solid rgba(255, 255, 255, 0.2);
            border-radius: 16px;
            color: rgba(255, 255, 255, 0.98);
            font-size: 1.05rem;
            font-weight: 600;
            cursor: pointer;
            margin-top: 12px;
            transition: all 0.3s ease;
            box-shadow: 0 4px 20px rgba(120, 119, 198, 0.3);
        }

        .submit-btn:active {
            transform: scale(0.98);
        }

        .toast {
            position: fixed;
            bottom: 120px;
            left: 50%;
            transform: translateX(-50%) translateY(100px) scale(0.9);
            background: linear-gradient(135deg, 
                rgba(255, 255, 255, 0.15) 0%, 
                rgba(255, 255, 255, 0.08) 100%);
            color: rgba(255, 255, 255, 0.95);
            padding: 18px 28px;
            border-radius: 20px;
            box-shadow: 0 15px 35px rgba(0, 0, 0, 0.25);
            z-index: 1000;
            opacity: 0;
            transition: all 0.4s ease;
            text-align: center;
            max-width: 90%;
            border: 1px solid rgba(255, 255, 255, 0.1);
            backdrop-filter: blur(20px);
            font-weight: 500;
        }

        .toast.show {
            transform: translateX(-50%) translateY(0) scale(1);
            opacity: 1;
        }

        .toast.success {
            border-left: 4px solid rgba(76, 175, 80, 0.8);
        }

        .toast.error {
            border-left: 4px solid rgba(244, 67, 54, 0.8);
        }

        @media (max-width: 480px) {
            .container {
                padding: 16px;
            }
            
            .track-title {
                font-size: 1.6rem;
            }
            
            .controls {
                gap: 16px;
            }
            
            .control-btn {
                width: 52px;
                height: 52px;
                font-size: 1.2rem;
            }
            
            .play-btn {
                width: 68px;
                height: 68px;
                font-size: 1.7rem;
            }
            
            .section-controls {
                gap: 10px;
            }
            
            .add-track-btn, .edit-playlist-btn {
                padding: 10px 16px;
                font-size: 0.9rem;
            }
            
            .modal-content {
                padding: 24px;
            }
        }

        .playlist-item.dragging {
            opacity: 0.5;
            background: linear-gradient(135deg, 
                rgba(120, 119, 198, 0.15) 0%, 
                rgba(255, 119, 230, 0.15) 100%);
            transform: scale(0.95);
        }

        .playlist-item.drag-over {
            border-top: 2px solid rgba(120, 119, 198, 0.9);
            margin-top: 2px;
        }

        @keyframes fadeInUp {
            from {
                opacity: 0;
                transform: translateY(20px);
            }
            to {
                opacity: 1;
                transform: translateY(0);
            }
        }

        .now-playing-card, .playlist-section {
            animation: fadeInUp 0.6s ease;
        }

        @keyframes colorFlow {
            0%, 100% { filter: hue-rotate(0deg); }
            50% { filter: hue-rotate(30deg); }
        }

        .play-btn, .progress, .add-track-btn, .edit-playlist-btn.active, .submit-btn {
            animation: colorFlow 10s infinite linear;
        }
    </style>
</head>
<body>
    <div class="container">
        <main>
            <div class="now-playing-card">
                <div class="track-info">
                    <h3 class="track-title" id="track-title">Выберите трек</h3>
                </div>

                <div class="progress-area">
                    <div class="progress-bar" id="progress-bar">
                        <div class="progress" id="progress"></div>
                    </div>
                    <div class="time-info">
                        <span id="current-time">0:00</span>
                        <span id="total-time">0:00</span>
                    </div>
                </div>

                <div class="controls">
                    <button class="control-btn" id="prev-btn">
                        <i class="fas fa-step-backward"></i>
                    </button>
                    <button class="control-btn play-btn" id="play-btn">
                        <i class="fas fa-play" id="play-icon"></i>
                    </button>
                    <button class="control-btn" id="next-btn">
                        <i class="fas fa-step-forward"></i>
                    </button>
                </div>
            </div>

            <section class="playlist-section">
                <div class="section-header">
                    <div class="section-controls">
                        <button class="add-track-btn" id="add-track-btn">
                            <i class="fas fa-plus"></i> Добавить
                        </button>
                        <button class="edit-playlist-btn" id="edit-playlist-btn">
                            <i class="fas fa-edit"></i> Редактировать
                        </button>
                    </div>
                </div>

                <ul class="playlist" id="playlist"></ul>

                <div class="empty-playlist" id="empty-playlist">
                    <i class="fas fa-music"></i>
                    <p>Плейлист пуст</p>
                    <p>Добавьте первый трек</p>
                </div>
            </section>
        </main>
    </div>

    <div class="modal" id="add-track-modal">
        <div class="modal-content">
            <div class="modal-header">
                <h3 class="modal-title">Добавить трек</h3>
                <button class="close-btn" id="close-modal-btn">
                    <i class="fas fa-times"></i>
                </button>
            </div>
            
            <div class="form-group">
                <label class="form-label">Ссылка на аудиофайл</label>
                <input type="text" id="track-url" class="form-input" placeholder="https://example.com/audio.mp3">
            </div>
            
            <div class="form-group">
                <label class="form-label">Название трека</label>
                <input type="text" id="track-name" class="form-input" placeholder="Название трека">
            </div>
            
            <button class="submit-btn" id="submit-track-btn">Добавить в плейлист</button>
        </div>
    </div>

    <div class="modal" id="edit-track-modal">
        <div class="modal-content">
            <div class="modal-header">
                <h3 class="modal-title">Редактировать трек</h3>
                <button class="close-btn" id="close-edit-modal-btn">
                    <i class="fas fa-times"></i>
                </button>
            </div>
            
            <div class="form-group">
                <label class="form-label">Ссылка на аудиофайл</label>
                <input type="text" id="edit-track-url" class="form-input" placeholder="https://example.com/audio.mp3">
            </div>
            
            <div class="form-group">
                <label class="form-label">Название трека</label>
                <input type="text" id="edit-track-name" class="form-input" placeholder="Название трека">
            </div>
            
            <button class="submit-btn" id="submit-edit-track-btn">Сохранить изменения</button>
        </div>
    </div>

    <div class="toast" id="toast"></div>

    <audio id="audio-player" preload="metadata"></audio>

    <script>
        const tg = window.Telegram?.WebApp;
        
        if (tg) {
            tg.expand();
            tg.BackButton.hide();
        }

        const audioPlayer = document.getElementById('audio-player');
        const playBtn = document.getElementById('play-btn');
        const playIcon = document.getElementById('play-icon');
        const prevBtn = document.getElementById('prev-btn');
        const nextBtn = document.getElementById('next-btn');
        const progressBar = document.getElementById('progress-bar');
        const progress = document.getElementById('progress');
        const currentTimeEl = document.getElementById('current-time');
        const totalTimeEl = document.getElementById('total-time');
        const trackTitle = document.getElementById('track-title');
        const playlistEl = document.getElementById('playlist');
        const emptyPlaylistEl = document.getElementById('empty-playlist');
        const addTrackBtn = document.getElementById('add-track-btn');
        const editPlaylistBtn = document.getElementById('edit-playlist-btn');
        const addTrackModal = document.getElementById('add-track-modal');
        const editTrackModal = document.getElementById('edit-track-modal');
        const closeModalBtn = document.getElementById('close-modal-btn');
        const closeEditModalBtn = document.getElementById('close-edit-modal-btn');
        const submitTrackBtn = document.getElementById('submit-track-btn');
        const submitEditTrackBtn = document.getElementById('submit-edit-track-btn');
        const trackUrlInput = document.getElementById('track-url');
        const trackNameInput = document.getElementById('track-name');
        const editTrackUrlInput = document.getElementById('edit-track-url');
        const editTrackNameInput = document.getElementById('edit-track-name');
        const toast = document.getElementById('toast');

        let currentTrackIndex = 0;
        let isPlaying = false;
        let playlist = [];
        let isEditMode = false;
        let currentEditIndex = -1;
        let dragStartIndex = -1;

        function loadPlaylist() {
            const savedPlaylist = localStorage.getItem('tgAudioPlayerPlaylist');
            if (savedPlaylist) {
                try {
                    playlist = JSON.parse(savedPlaylist);
                    renderPlaylist();
                    if (playlist.length > 0) {
                        loadTrack(currentTrackIndex, false);
                    }
                } catch (e) {
                    playlist = [];
                }
            }
            updateEmptyPlaylistVisibility();
        }

        function savePlaylist() {
            localStorage.setItem('tgAudioPlayerPlaylist', JSON.stringify(playlist));
        }

        function renderPlaylist() {
            playlistEl.innerHTML = '';
            
            playlist.forEach((track, index) => {
                const li = document.createElement('li');
                li.className = `playlist-item ${index === currentTrackIndex ? 'active' : ''}`;
                li.dataset.index = index;
                li.style.animation = 'fadeInUp 0.4s ease-out';
                
                if (isEditMode) {
                    li.innerHTML = `
                        <div class="playlist-icon">
                            <i class="fas fa-music"></i>
                        </div>
                        <div class="playlist-info">
                            <div class="playlist-title">${track.name || 'Без названия'}</div>
                        </div>
                        <div class="playlist-controls">
                            <button class="edit-btn" data-index="${index}">
                                <i class="fas fa-edit"></i>
                            </button>
                            <button class="delete-btn" data-index="${index}">
                                <i class="fas fa-trash"></i>
                            </button>
                            <button class="drag-handle" data-index="${index}">
                                <i class="fas fa-bars"></i>
                            </button>
                        </div>
                    `;
                } else {
                    li.innerHTML = `
                        <div class="playlist-icon">
                            <i class="fas fa-music"></i>
                        </div>
                        <div class="playlist-info">
                            <div class="playlist-title">${track.name || 'Без названия'}</div>
                        </div>
                        <div class="playlist-controls">
                        </div>
                    `;
                }
                
                li.addEventListener('click', (e) => {
                    if (!e.target.closest('.playlist-controls')) {
                        playTrack(index);
                    }
                });
                
                if (isEditMode) {
                    const dragHandle = li.querySelector('.drag-handle');
                    const editBtn = li.querySelector('.edit-btn');
                    const deleteBtn = li.querySelector('.delete-btn');
                    
                    dragHandle.addEventListener('mousedown', (e) => startDrag(e, index));
                    dragHandle.addEventListener('touchstart', (e) => startDrag(e, index));
                    
                    editBtn.addEventListener('click', (e) => {
                        e.stopPropagation();
                        editTrack(index);
                    });
                    
                    deleteBtn.addEventListener('click', (e) => {
                        e.stopPropagation();
                        deleteTrack(index);
                    });
                }
                
                playlistEl.appendChild(li);
            });
            
            updateEmptyPlaylistVisibility();
        }

        function updateEmptyPlaylistVisibility() {
            emptyPlaylistEl.style.display = playlist.length === 0 ? 'block' : 'none';
        }

        function addTrack(url, name) {
            if (!url) {
                showToast('Введите ссылку на аудиофайл', 'error');
                return;
            }
            
            if (!url.startsWith('http://') && !url.startsWith('https://')) {
                showToast('Неверный формат ссылки', 'error');
                return;
            }
            
            if (playlist.some(track => track.url === url)) {
                showToast('Этот трек уже есть в плейлисте', 'error');
                return;
            }
            
            playlist.push({
                url,
                name: name || 'Без названия'
            });
            
            savePlaylist();
            renderPlaylist();
            
            if (playlist.length === 1) {
                playTrack(0);
            }
            
            showToast('Трек добавлен в плейлист', 'success');
            closeModal();
            trackUrlInput.value = '';
            trackNameInput.value = '';
        }

        function editTrack(index) {
            if (index < 0 || index >= playlist.length) return;
            
            const track = playlist[index];
            currentEditIndex = index;
            
            editTrackUrlInput.value = track.url;
            editTrackNameInput.value = track.name;
            
            editTrackModal.classList.add('active');
        }

        function saveEditedTrack() {
            if (currentEditIndex < 0 || currentEditIndex >= playlist.length) return;
            
            const url = editTrackUrlInput.value.trim();
            const name = editTrackNameInput.value.trim();
            
            if (!url) {
                showToast('Введите ссылку на аудиофайл', 'error');
                return;
            }
            
            if (!url.startsWith('http://') && !url.startsWith('https://')) {
                showToast('Неверный формат ссылки', 'error');
                return;
            }
            
            if (playlist.some((track, index) => track.url === url && index !== currentEditIndex)) {
                showToast('Этот трек уже есть в плейлисте', 'error');
                return;
            }
            
            playlist[currentEditIndex] = {
                url,
                name: name || 'Без названия'
            };
            
            savePlaylist();
            renderPlaylist();
            
            if (currentEditIndex === currentTrackIndex) {
                loadTrack(currentTrackIndex, false);
            }
            
            showToast('Трек обновлен', 'success');
            closeEditModal();
        }

        function deleteTrack(index) {
            if (index < 0 || index >= playlist.length) return;
            
            playlist.splice(index, 1);
            
            if (currentTrackIndex >= index && currentTrackIndex > 0) {
                currentTrackIndex--;
            }
            
            if (playlist.length === 0) {
                audioPlayer.src = '';
                trackTitle.textContent = 'Выберите трек';
                isPlaying = false;
                playIcon.className = 'fas fa-play';
                progress.style.width = '0%';
                currentTimeEl.textContent = '0:00';
                totalTimeEl.textContent = '0:00';
            } else {
                playTrack(currentTrackIndex);
            }
            
            savePlaylist();
            renderPlaylist();
            showToast('Трек удален', 'success');
        }

        function toggleEditMode() {
            isEditMode = !isEditMode;
            
            if (isEditMode) {
                editPlaylistBtn.classList.add('active');
                editPlaylistBtn.innerHTML = '<i class="fas fa-check"></i> Готово';
            } else {
                editPlaylistBtn.classList.remove('active');
                editPlaylistBtn.innerHTML = '<i class="fas fa-edit"></i> Редактировать';
            }
            
            renderPlaylist();
        }

        function startDrag(e, index) {
            e.preventDefault();
            dragStartIndex = index;
            
            const li = playlistEl.children[index];
            li.classList.add('dragging');
            
            document.addEventListener('mousemove', handleDrag);
            document.addEventListener('mouseup', stopDrag);
            document.addEventListener('touchmove', handleDrag);
            document.addEventListener('touchend', stopDrag);
        }

        function handleDrag(e) {
            e.preventDefault();
            
            const clientY = e.clientY || (e.touches && e.touches[0].clientY);
            if (!clientY) return;
            
            const playlistItems = Array.from(playlistEl.children);
            const draggingItem = playlistEl.children[dragStartIndex];
            
            playlistItems.forEach(item => item.classList.remove('drag-over'));
            
            const dragOverItem = playlistItems.find(item => {
                const rect = item.getBoundingClientRect();
                return clientY > rect.top && clientY < rect.bottom && item !== draggingItem;
            });
            
            if (dragOverItem) {
                dragOverItem.classList.add('drag-over');
            }
        }

        function stopDrag(e) {
            e.preventDefault();
            
            const clientY = e.clientY || (e.changedTouches && e.changedTouches[0].clientY);
            if (!clientY || dragStartIndex === -1) return;
            
            const playlistItems = Array.from(playlistEl.children);
            playlistItems.forEach(item => {
                item.classList.remove('dragging');
                item.classList.remove('drag-over');
            });
            
            let dragEndIndex = -1;
            for (let i = 0; i < playlistItems.length; i++) {
                const rect = playlistItems[i].getBoundingClientRect();
                if (clientY > rect.top && clientY < rect.bottom) {
                    dragEndIndex = i;
                    break;
                }
            }
            
            if (dragEndIndex !== -1 && dragEndIndex !== dragStartIndex) {
                const [draggedTrack] = playlist.splice(dragStartIndex, 1);
                playlist.splice(dragEndIndex, 0, draggedTrack);
                
                if (currentTrackIndex === dragStartIndex) {
                    currentTrackIndex = dragEndIndex;
                } else if (currentTrackIndex > dragStartIndex && currentTrackIndex <= dragEndIndex) {
                    currentTrackIndex--;
                } else if (currentTrackIndex < dragStartIndex && currentTrackIndex >= dragEndIndex) {
                    currentTrackIndex++;
                }
                
                savePlaylist();
                renderPlaylist();
                showToast('Порядок треков изменен', 'success');
            }
            
            dragStartIndex = -1;
            
            document.removeEventListener('mousemove', handleDrag);
            document.removeEventListener('mouseup', stopDrag);
            document.removeEventListener('touchmove', handleDrag);
            document.removeEventListener('touchend', stopDrag);
        }

        function loadTrack(index, autoPlay = true) {
            if (index < 0 || index >= playlist.length) return;
            
            const track = playlist[index];
            trackTitle.textContent = track.name;
            audioPlayer.src = track.url;
            audioPlayer.volume = 1.0;
            currentTrackIndex = index;
            renderPlaylist();
            
            if (autoPlay) {
                setTimeout(() => {
                    playAudio();
                }, 100);
            }
        }

        function playTrack(index) {
            if (index < 0 || index >= playlist.length) return;
            loadTrack(index);
        }

        function playAudio() {
            if (playlist.length === 0) return;
            
            if (audioPlayer.paused) {
                audioPlayer.play().then(() => {
                    isPlaying = true;
                    playIcon.className = 'fas fa-pause';
                }).catch(e => {
                    showToast('Ошибка воспроизведения аудио', 'error');
                });
            } else {
                audioPlayer.pause();
                isPlaying = false;
                playIcon.className = 'fas fa-play';
            }
        }

        function nextTrack() {
            if (playlist.length === 0) return;
            
            let nextIndex = currentTrackIndex + 1;
            if (nextIndex >= playlist.length) {
                nextIndex = 0;
            }
            
            playTrack(nextIndex);
        }

        function prevTrack() {
            if (playlist.length === 0) return;
            
            let prevIndex = currentTrackIndex - 1;
            if (prevIndex < 0) {
                prevIndex = playlist.length - 1;
            }
            
            playTrack(prevIndex);
        }

        function formatTime(seconds) {
            if (isNaN(seconds)) return '0:00';
            
            const mins = Math.floor(seconds / 60);
            const secs = Math.floor(seconds % 60);
            return `${mins}:${secs < 10 ? '0' : ''}${secs}`;
        }

        function updateProgress() {
            const { currentTime, duration } = audioPlayer;
            
            if (isNaN(duration)) return;
            
            const progressPercent = (currentTime / duration) * 100;
            progress.style.width = `${progressPercent}%`;
            
            currentTimeEl.textContent = formatTime(currentTime);
            totalTimeEl.textContent = formatTime(duration);
        }

        function setProgress(e) {
            const width = this.clientWidth;
            const clickX = e.offsetX;
            const duration = audioPlayer.duration;
            
            if (isNaN(duration)) return;
            
            audioPlayer.currentTime = (clickX / width) * duration;
        }

        function showToast(message, type = 'info') {
            toast.textContent = message;
            toast.className = `toast ${type}`;
            toast.classList.add('show');
            
            setTimeout(() => {
                toast.classList.remove('show');
            }, 3000);
        }

        function openModal() {
            addTrackModal.classList.add('active');
        }

        function closeModal() {
            addTrackModal.classList.remove('active');
        }

        function closeEditModal() {
            editTrackModal.classList.remove('active');
            currentEditIndex = -1;
            editTrackUrlInput.value = '';
            editTrackNameInput.value = '';
        }

        function init() {
            loadPlaylist();
            
            playBtn.addEventListener('click', playAudio);
            prevBtn.addEventListener('click', prevTrack);
            nextBtn.addEventListener('click', nextTrack);
            
            audioPlayer.addEventListener('timeupdate', updateProgress);
            audioPlayer.addEventListener('ended', nextTrack);
            
            progressBar.addEventListener('click', setProgress);
            
            addTrackBtn.addEventListener('click', openModal);
            editPlaylistBtn.addEventListener('click', toggleEditMode);
            
            closeModalBtn.addEventListener('click', closeModal);
            closeEditModalBtn.addEventListener('click', closeEditModal);
            
            submitTrackBtn.addEventListener('click', () => {
                addTrack(
                    trackUrlInput.value.trim(),
                    trackNameInput.value.trim()
                );
            });
            
            submitEditTrackBtn.addEventListener('click', saveEditedTrack);
            
            addTrackModal.addEventListener('click', (e) => {
                if (e.target === addTrackModal) {
                    closeModal();
                }
            });
            
            editTrackModal.addEventListener('click', (e) => {
                if (e.target === editTrackModal) {
                    closeEditModal();
                }
            });
            
            trackUrlInput.addEventListener('keypress', (e) => {
                if (e.key === 'Enter') {
                    addTrack(
                        trackUrlInput.value.trim(),
                        trackNameInput.value.trim()
                    );
                }
            });
            
            editTrackUrlInput.addEventListener('keypress', (e) => {
                if (e.key === 'Enter') {
                    saveEditedTrack();
                }
            });
            
            if (navigator.vibrate) {
                const buttons = document.querySelectorAll('button');
                buttons.forEach(btn => {
                    btn.addEventListener('click', () => {
                        navigator.vibrate(10);
                    });
                });
            }
        }

        document.addEventListener('DOMContentLoaded', init);
    </script>
</body>
</html>
