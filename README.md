
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Minimal Music Player</title>
<style>
  body {
    font-family: Arial, sans-serif;
    background: #d1d5db;
    display: flex;
    justify-content: center;
    align-items: center;
    height: 100vh;
    margin: 0;
  }
  .player {
    background: #e5e7eb;
    padding: 20px;
    border-radius: 15px;
    width: 300px;
    text-align: center;
    box-shadow: 0 5px 15px rgba(0,0,0,0.1);
    display: flex;
    flex-direction: column;
  }
  .song-info {
    text-align: left;
    margin-bottom: 15px;
  }
  .song-title {
    font-weight: bold;
    font-size: 18px;
  }
  .singer {
    font-size: 14px;
    color: #6b7280;
  }
  .lyrics {
    font-size: 12px;
    color: #4b5563;
    margin: 10px 0 15px 0;
  }
  .controls {
    background: #111827;
    padding: 10px;
    border-radius: 12px;
  }
  .buttons {
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin-bottom: 10px;
  }
  .buttons button {
    background: none;
    border: none;
    color: #fff;
    font-size: 18px;
    cursor: pointer;
  }
  .progress-container {
    background: #374151;
    height: 5px;
    border-radius: 5px;
    overflow: hidden;
    cursor: pointer;
  }
  .progress {
    background: #7c5cff;
    height: 100%;
    width: 0%;
  }
  #file-input {
    margin: 10px 0;
    width: 100%;
  }
  .footer {
    font-size: 10px;
    color: #6b7280;
    margin-top: 10px;
  }
</style>
</head>
<body>

<div class="player">
  <div class="song-info">
    <div class="song-title" id="song-title">Song Title</div>
    <div class="singer" id="singer">Singer Name</div>
  </div>
  
  <div class="lyrics" id="lyrics">Your lyric goes here</div>
  
  <!-- File input inside the player -->
  <input type="file" id="file-input" accept="audio/*">
  
  <div class="controls">
    <div class="buttons">
      <button id="shuffle">🔀</button>
      <button id="prev">⏮️</button>
      <button id="play">▶️</button>
      <button id="next">⏭️</button>
      <button id="repeat">🔁</button>
    </div>
    <div class="progress-container" id="progress-container">
      <div class="progress" id="progress"></div>
    </div>
  </div>

  <!-- Footer -->
  <div class="footer">Made with ❤ by Armeen</div>
  
  <audio id="audio" src=""></audio>
</div>

<script>
const audio = document.getElementById('audio');
const playBtn = document.getElementById('play');
const prevBtn = document.getElementById('prev');
const nextBtn = document.getElementById('next');
const progress = document.getElementById('progress');
const progressContainer = document.getElementById('progress-container');
const fileInput = document.getElementById('file-input');

let songs = [
  {title: "Song 1", singer: "Singer 1", src: "song1.mp3", lyrics: "Lyrics of Song 1..."},
  {title: "Song 2", singer: "Singer 2", src: "song2.mp3", lyrics: "Lyrics of Song 2..."}
];

let currentSong = 0;

// Load song
function loadSong(index) {
  document.getElementById('song-title').textContent = songs[index].title;
  document.getElementById('singer').textContent = songs[index].singer;
  document.getElementById('lyrics').textContent = songs[index].lyrics;
  audio.src = songs[index].src;
}

// Play/Pause
function playSong() {
  audio.play();
  playBtn.textContent = '⏸️';
}
function pauseSong() {
  audio.pause();
  playBtn.textContent = '▶️';
}

playBtn.addEventListener('click', ()=>{
  audio.paused ? playSong() : pauseSong();
});

// Next/Prev
nextBtn.addEventListener('click', ()=>{
  if (currentSong === -1) return;
  currentSong = (currentSong+1) % songs.length;
  loadSong(currentSong);
  playSong();
});
prevBtn.addEventListener('click', ()=>{
  if (currentSong === -1) return;
  currentSong = (currentSong-1+songs.length) % songs.length;
  loadSong(currentSong);
  playSong();
});

// Progress bar
audio.addEventListener('timeupdate', ()=>{
  const progressPercent = (audio.currentTime / audio.duration)*100;
  progress.style.width = `${progressPercent}%`;
});

// Seek
progressContainer.addEventListener('click', (e)=>{
  const width = progressContainer.clientWidth;
  const clickX = e.offsetX;
  audio.currentTime = (clickX/width)*audio.duration;
});

// Load song from device
fileInput.addEventListener('change', (e) => {
  const file = e.target.files[0];
  if (file) {
    document.getElementById('song-title').textContent = file.name;
    document.getElementById('singer').textContent = "Unknown";
    document.getElementById('lyrics').textContent = "No lyrics available";
    audio.src = URL.createObjectURL(file);
    currentSong = -1; // disables next/prev
    playSong();
  }
});

// Load first song
loadSong(currentSong);
</script>

</body>
</html>
# MUSIC-PLAYER
