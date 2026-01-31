<html lang="ru">
<head>
  <meta charset="UTF-8">
  <title>Telegram Audio Player</title>
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <script src="https://telegram.org/js/telegram-web-app.js"></script>
  <style>
    body {
      font-family: sans-serif;
      background: #121212;
      color: #fff;
      text-align: center;
      padding: 20px;
    }
    audio {
      width: 100%;
      margin: 20px 0;
    }
    input, button {
      padding: 10px;
      margin: 5px;
      border-radius: 5px;
      border: none;
    }
    input {
      width: 70%;
    }
    button {
      background: #0088cc;
      color: #fff;
      cursor: pointer;
    }
    ul {
      list-style: none;
      padding: 0;
    }
    li {
      margin: 5px 0;
      cursor: pointer;
    }
  </style>
</head>
<body>
  <h2>🎵 Telegram Mini App Player</h2>

  <audio id="player" controls autoplay playsinline></audio>

  <div>
    <input id="streamUrl" type="text" placeholder="Введите ссылку на потоковое аудио">
    <button onclick="addStream()">Добавить</button>
  </div>

  <h3>📻 Список потоков</h3>
  <ul id="playlist"></ul>

  <script>
    const player = document.getElementById('player');
    const playlistEl = document.getElementById('playlist');
    let playlist = [];

    function addStream() {
      const url = document.getElementById('streamUrl').value.trim();
      if (url) {
        playlist.push(url);
        renderPlaylist();
        document.getElementById('streamUrl').value = '';
      }
    }

    function renderPlaylist() {
      playlistEl.innerHTML = '';
      playlist.forEach((url, index) => {
        const li = document.createElement('li');
        li.textContent = url;
        li.onclick = () => playStream(index);
        playlistEl.appendChild(li);
      });
    }

    function playStream(index) {
      player.src = playlist[index];
      player.play();
    }

    // Telegram WebApp API
    Telegram.WebApp.ready();
    Telegram.WebApp.expand();

    // Фоновое воспроизведение
    document.addEventListener("visibilitychange", () => {
      if (document.hidden) {
        console.log("Mini App свернут, поток продолжает играть");
      }
    });
  </script>
</body>
</html>
