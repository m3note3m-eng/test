<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Веб-радио "Сталица"</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
        }
        
        body {
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
            background: linear-gradient(135deg, #1a1a2e 0%, #16213e 100%);
            color: #f1f1f1;
            padding: 20px;
        }
        
        .player-container {
            width: 100%;
            max-width: 400px;
            background: rgba(25, 25, 40, 0.9);
            border-radius: 24px;
            padding: 30px;
            box-shadow: 0 15px 35px rgba(0, 0, 0, 0.5);
            border: 1px solid rgba(255, 255, 255, 0.1);
        }
        
        .station-info {
            text-align: center;
            margin-bottom: 40px;
        }
        
        .station-logo {
            width: 140px;
            height: 140px;
            background: linear-gradient(45deg, #2d4059, #222831);
            border-radius: 50%;
            margin: 0 auto 25px;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 50px;
            color: #e94560;
            border: 6px solid #0f3460;
        }
        
        h1 {
            font-size: 32px;
            margin-bottom: 10px;
            color: #ffffff;
            letter-spacing: 0.5px;
        }
        
        .station-subtitle {
            color: #a0a0c0;
            font-size: 18px;
            margin-bottom: 15px;
        }
        
        .status-indicator {
            display: inline-block;
            width: 14px;
            height: 14px;
            background-color: #4a4a6d;
            border-radius: 50%;
            margin-right: 10px;
            transition: background-color 0.3s;
        }
        
        .status-indicator.active {
            background-color: #00ff88;
            box-shadow: 0 0 12px #00ff88;
        }
        
        .status-text {
            font-size: 16px;
            color: #a0a0c0;
        }
        
        .controls {
            display: flex;
            justify-content: center;
            margin-bottom: 30px;
        }
        
        .play-button {
            width: 100px;
            height: 100px;
            border-radius: 50%;
            background: linear-gradient(45deg, #e94560, #ff2e63);
            border: none;
            color: white;
            font-size: 36px;
            cursor: pointer;
            transition: all 0.3s;
            box-shadow: 0 10px 25px rgba(233, 69, 96, 0.4);
        }
        
        .play-button:hover {
            transform: scale(1.08);
            box-shadow: 0 12px 30px rgba(233, 69, 96, 0.6);
        }
        
        .play-button:active {
            transform: scale(0.98);
        }
        
        @media (max-width: 480px) {
            .player-container {
                padding: 25px;
                border-radius: 20px;
            }
            
            .station-logo {
                width: 120px;
                height: 120px;
                font-size: 42px;
            }
            
            h1 {
                font-size: 28px;
            }
            
            .play-button {
                width: 90px;
                height: 90px;
                font-size: 32px;
            }
        }
    </style>
</head>
<body>
    <div class="player-container">
        <div class="station-info">
            <div class="station-logo">📻</div>
            <h1>Сталица</h1>
            <p class="station-subtitle">Онлайн-радио</p>
            <p>
                <span id="statusIndicator" class="status-indicator"></span>
                <span id="statusText" class="status-text">Неактивно</span>
            </p>
        </div>
        
        <div class="controls">
            <button id="playButton" class="play-button">▶</button>
        </div>
    </div>

    <script>
        document.addEventListener('DOMContentLoaded', function() {
            // Элементы управления
            const playButton = document.getElementById('playButton');
            const statusIndicator = document.getElementById('statusIndicator');
            const statusText = document.getElementById('statusText');
            
            // URL потока
            const streamUrl = 'https://media2.datacenter.by/stream/stalica/stream';
            
            // Создаем аудио элемент
            const audio = new Audio();
            audio.src = streamUrl;
            audio.volume = 0.7; // Фиксированная громкость
            
            // Переменная состояния воспроизведения
            let isPlaying = false;
            
            // Обработчик клика по кнопке воспроизведения
            playButton.addEventListener('click', function() {
                if (isPlaying) {
                    // Останавливаем воспроизведение
                    audio.pause();
                    playButton.textContent = '▶';
                    statusIndicator.classList.remove('active');
                    statusText.textContent = 'Приостановлено';
                } else {
                    // Начинаем воспроизведение
                    audio.play()
                        .then(() => {
                            playButton.textContent = '⏸';
                            statusIndicator.classList.add('active');
                            statusText.textContent = 'Воспроизводится';
                            isPlaying = true;
                        })
                        .catch(error => {
                            console.error('Ошибка воспроизведения:', error);
                            statusText.textContent = 'Ошибка подключения';
                        });
                }
            });
            
            // Обработчики событий аудио
            audio.addEventListener('playing', function() {
                isPlaying = true;
                playButton.textContent = '⏸';
                statusIndicator.classList.add('active');
                statusText.textContent = 'Воспроизводится';
            });
            
            audio.addEventListener('pause', function() {
                isPlaying = false;
                playButton.textContent = '▶';
                statusIndicator.classList.remove('active');
                statusText.textContent = 'Приостановлено';
            });
            
            audio.addEventListener('ended', function() {
                isPlaying = false;
                playButton.textContent = '▶';
                statusIndicator.classList.remove('active');
                statusText.textContent = 'Завершено';
            });
            
            audio.addEventListener('error', function() {
                isPlaying = false;
                playButton.textContent = '▶';
                statusIndicator.classList.remove('active');
                statusText.textContent = 'Ошибка потока';
            });
        });
    </script>
</body>
</html>
