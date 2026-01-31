<!DOCTYPE html>
<html lang="ru">
<head>
  <meta charset="UTF-8">
  <title>Telegram Audio Player</title>
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <script src="https://telegram.org/js/telegram-web-app.js"></script>
  <style>
    body {
      font-family: sans-serif;
      text-align: center;
      background: #121212;
      color: #fff;
    }
    audio {
      width: 90%;
      margin-top: 20px;
    }
    button {
      margin-top: 15px;
      padding: 10px 20px;
      background: #0088cc;
      border: none;
      color: #fff;
      border-radius: 5px;
      cursor: pointer;
    }
  </style>
</head>
<body>
  <h2>🎵 Telegram Mini App Player</h2>
  <audio id="player" controls autoplay playsinline>
    <source src="https://your-stream-url.com/stream.mp3" type="audio/mpeg">
    Ваш браузер не поддерживает аудио.
  </audio>
  <br>
  <button onclick="togglePlay()">▶️ / ⏸️</button>

  <script>
    const player = document.getElementById('player');

    function togglePlay() {
      if (player.paused) {
        player.play();
      } else {
        player.pause();
      }
    }

    // Telegram WebApp API
    Telegram.WebApp.ready();
    Telegram.WebApp.expand(); // разворачивает окно

    // Обработка фонового режима
    document.addEventListener("visibilitychange", () => {
      if (document.hidden) {
        // можно оставить playing, чтобы не останавливать
        console.log("App в фоне, аудио продолжает играть");
      }
    });
  </script>
</body>
</html>

