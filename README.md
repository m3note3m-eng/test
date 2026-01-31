<html lang="ru">
<head>
  <meta charset="UTF-8">
  <title>Telegram Mini Audio Player</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      background-color: #f0f0f0;
      margin: 0;
      padding: 20px;
    }
    .container {
      background: #fff;
      border-radius: 10px;
      padding: 15px;
      box-shadow: 0 0 10px rgba(0,0,0,0.2);
    }
    input[type="text"] {
      width: 100%;
      padding: 8px;
      border-radius: 5px;
      border: 1px solid #ccc;
      margin-bottom: 10px;
    }
    button {
      background-color: #0088cc;
      color: #fff;
      border: none;
      padding: 8px 12px;
      border-radius: 5px;
      cursor: pointer;
    }
    button:hover {
      background-color: #006699;
    }
    audio {
      width: 100%;
      margin-top: 10px;
    }
  </style>
</head>
<body>
  <div class="container">
    <h3>🎵 Мини‑плеер</h3>
    <input type="text" id="audioUrl" placeholder="https://example.com/audio.mp3">
    <button onclick="playAudio()">▶ Воспроизвести</button>
    <audio id="player" controls></audio>
  </div>

  <!-- Подключение Telegram WebApp API -->
  <script src="https://telegram.org/js/telegram-web-app.js"></script>
  <script>
    function playAudio() {
      const url = document.getElementById('audioUrl').value;
      const player = document.getElementById('player');
      if (url) {
        player.src = url;
        player.play();
        // Отправим событие в Telegram
        Telegram.WebApp.sendData("Воспроизведение: " + url);
      } else {
        alert("Введите корректную ссылку на аудио!");
      }
    }

    // Настройка темы Telegram
    Telegram.WebApp.ready();
    document.body.style.backgroundColor = Telegram.WebApp.backgroundColor;
  </script>
</body>
</html>
