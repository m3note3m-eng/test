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

  <!-- Telegram WebApp API -->
  <script src="https://telegram.org/js/telegram-web-app.js"></script>
  <script>
    const player = document.getElementById('player');

    function playAudio() {
      const url = document.getElementById('audioUrl').value;
      if (url) {
        player.src = url;
        player.play().then(() => {
          // Сообщаем в Telegram, что началось воспроизведение
          Telegram.WebApp.sendData("Фоновое воспроизведение: " + url);
        }).catch(err => {
          alert("Ошибка воспроизведения: " + err);
        });
      } else {
        alert("Введите корректную ссылку на аудио!");
      }
    }

    // Настройка Telegram WebApp
    Telegram.WebApp.ready();
    document.body.style.backgroundColor = Telegram.WebApp.backgroundColor;

    // Включаем фоновое воспроизведение
    player.setAttribute("playsinline", "true"); // для iOS
    player.loop = true; // при желании можно зациклить
  </script>
</body>
</html>
