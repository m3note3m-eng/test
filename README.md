<html lang="ru">
<head>
  <meta charset="UTF-8">
  <title>Glass Audio Player</title>
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <style>
    body {
      font-family: Arial, sans-serif;
      background: url('https://picsum.photos/1200/800?blur') no-repeat center center fixed;
      background-size: cover;
      display: flex;
      justify-content: center;
      align-items: center;
      height: 100vh;
      margin: 0;
    }

    .player {
      width: 350px;
      padding: 20px;
      border-radius: 20px;
      background: rgba(255, 255, 255, 0.15);
      backdrop-filter: blur(12px);
      box-shadow: 0 8px 32px rgba(0,0,0,0.3);
      color: #fff;
    }

    .player h2 {
      text-align: center;
      margin-bottom: 15px;
    }

    .controls {
      display: flex;
      justify-content: center;
      gap: 10px;
      margin: 10px 0;
    }

    button {
      padding: 8px 14px;
      border: none;
      border-radius: 10px;
      background: rgba(255,255,255,0.2);
      color: #fff;
      cursor: pointer;
      transition: background 0.3s;
    }

    button:hover {
      background: rgba(255,255,255,0.4);
    }

    input {
      width: 100%;
      padding: 8px;
      border-radius: 10px;
      border: none;
      margin-bottom: 10px;
      background: rgba(255,255,255,0.2);
      color: #fff;
    }

    ul {
      list-style: none;
      padding: 0;
      margin: 0;
      max-height: 150px;
      overflow-y: auto;
    }

    li {
      padding: 6px;
      border-radius: 8px;
      background: rgba(255,255,255,0.1);
      margin-bottom: 5px;
      cursor: pointer;
    }

    li.active {
      background: rgba(255,255,255,0.3);
    }
  </style>
</head>
<body>
  <div class="player">
    <h2>🎵 Glass Player</h2>
    <audio id="audio" controls style="width:100%;"></audio>

    <div class="controls">
      <button onclick="prevTrack()">⏮️</button>
      <button onclick="playPause()">⏯️</button>
      <button onclick="nextTrack()">⏭️</button>
    </div>

    <input type="text" id="urlInput" placeholder="Вставьте ссылку на потоковое аудио">
    <button onclick="addTrack()">Добавить в плейлист</button>

    <ul id="playlist"></ul>
  </div>

  <script>
    const audio = document.getElementById('audio');
    const playlistEl = document.getElementById('playlist');
    let playlist = [];
    let currentIndex = -1;

    function addTrack() {
      const url = document.getElementById('urlInput').value.trim();
      if (url) {
        playlist.push(url);
        renderPlaylist();
        document.getElementById('urlInput').value = '';
      }
    }

    function renderPlaylist() {
      playlistEl.innerHTML = '';
      playlist.forEach((track, index) => {
        const li = document.createElement('li');
        li.textContent = track;
        li.onclick = () => playTrack(index);
        if (index === currentIndex) li.classList.add('active');
        playlistEl.appendChild(li);
      });
    }

    function playTrack(index) {
      currentIndex = index;
      audio.src = playlist[index];
      audio.play();
      renderPlaylist();
    }

    function playPause() {
      if (audio.paused) {
        audio.play();
      } else {
        audio.pause();
      }
    }

    function nextTrack() {
      if (playlist.length > 0) {
        currentIndex = (currentIndex + 1) % playlist.length;
        playTrack(currentIndex);
      }
    }

    function prevTrack() {
      if (playlist.length > 0) {
        currentIndex = (currentIndex - 1 + playlist.length) % playlist.length;
        playTrack(currentIndex);
      }
    }
  </script>
</body>
</html>

