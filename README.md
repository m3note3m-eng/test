<html lang="ru">
<head>
  <meta charset="UTF-8">
  <title>Telegram Radio Stream</title>
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
    h3 {
      margin-top: 0;
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
      margin-right: 5px;
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
    <h3>📻 Радио‑стрим</h3>
    <input type="text" id="streamUrl" placeholder="http://radio.example.com:8000/stream.mp3">
    <button onclick="startStream()">▶ Старт</button>
    <button onclick="stopStream()">⏹ Стоп</button>
    <audio id="radioPlayer" controls autoplay playsinline></audio>
  </div>

  <!-- Telegram WebApp API -->
  <script src="https://telegram.org/js/telegram-web-app.js"></script>
  <script>
    const player = document.getElementById('radioPlayer');

    function startStream() {
      const url = document.getElementById('streamUrl').value;
      if (url) {
        player.src = url;
        player.play().then(() => {
          Telegram.WebApp.sendData("Стрим запущен: " + url);
        }).catch(err => {
          alert("Ошибка воспроизведения: " + err);
        });
      } else {
        alert("Введите ссылку на радио‑стрим!");
      }
    }

    function stopStream() {
      player.pause();
      player.src = "";
      Telegram.WebApp.sendData("Стрим остановлен");
    }

    // Настройка Telegram WebApp
    Telegram.WebApp.ready();
    document.body.style.backgroundColor = Telegram.WebApp.backgroundColor;
  </script>
</body>
</html>
