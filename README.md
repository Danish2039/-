<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Custom Countdown Timer</title>
  <style>
    @import url('https://fonts.googleapis.com/css2?family=Lemon+Milk&display=swap');

    :root {
      --background-color: #fdf6e3;
      --text-color: #333;
      --button-bg: #222;
      --button-hover-bg: #555;
    }

    body {
      margin: 0;
      display: flex;
      flex-direction: column;
      justify-content: center;
      align-items: center;
      min-height: 100vh;
      background: var(--background-color) url('https://www.transparenttextures.com/patterns/lined-paper.png');
      color: var(--text-color);
      font-family: 'Lemon Milk', sans-serif;
      box-sizing: border-box;
      overflow: hidden;
      position: relative;
    }

    .time {
      font-size: calc(10vw + 1rem);
      font-weight: bold;
      letter-spacing: 4px;
      margin: 20px 0;
      color: var(--text-color);
      text-align: center;
    }

    .buttons {
      display: flex;
      justify-content: center;
      gap: 20px;
      margin-top: 20px;
    }

    button {
      font-size: 1rem;
      padding: 10px 20px;
      background-color: var(--button-bg);
      color: #f4f4f4;
      border: none;
      border-radius: 10px;
      cursor: pointer;
      transition: background-color 0.3s, color 0.3s;
    }

    button:hover {
      background-color: var(--button-hover-bg);
      color: white;
    }

    .input-container {
      margin-bottom: 20px;
      text-align: center;
    }

    .input-container input {
      font-size: 1.5rem;
      padding: 10px;
      width: 100%;
      max-width: 200px;
      text-align: center;
      border: 2px solid var(--text-color);
      border-radius: 5px;
      box-sizing: border-box;
    }

    .theme-selector {
      position: absolute;
      top: 10px;
      right: 10px;
      text-align: right;
    }

    .theme-selector select {
      font-size: 1rem;
      padding: 10px;
      border: 2px solid var(--text-color);
      border-radius: 5px;
    }

    .name-date {
      position: absolute;
      top: 10px;
      left: 10px;
      font-size: 1rem;
      text-align: left;
    }

    .name-date input {
      font-size: 1rem;
      padding: 5px;
      margin: 5px 0;
      border: 1px solid var(--text-color);
      border-radius: 3px;
      box-sizing: border-box;
      width: calc(100% - 20px);
      max-width: 150px;
    }

    .usage-tracker {
      position: absolute;
      bottom: 10px;
      font-size: 1rem;
      color: var(--text-color);
      text-align: center;
    }

    .doodles {
      position: absolute;
      width: 100%;
      height: 100%;
      top: 0;
      left: 0;
      pointer-events: none;
    }

    .doodles img {
      position: absolute;
      height: 80px;
      width: auto;
      opacity: 0.2;
    }

    @media (max-width: 768px) {
      .time {
        font-size: calc(8vw + 1rem);
      }

      .buttons button {
        padding: 10px;
        font-size: 0.9rem;
      }

      .doodles img {
        height: 60px;
      }
    }

    @media (max-width: 480px) {
      .time {
        font-size: calc(6vw + 1rem);
        letter-spacing: 2px;
      }

      .buttons button {
        padding: 8px;
        font-size: 0.8rem;
      }

      .doodles img {
        height: 40px;
      }
    }
  </style>
</head>
<body>
  <div class="name-date">
    <label for="name">Name:</label>
    <input type="text" id="name" placeholder="Enter your name">
    <br>
    <label for="date">Date:</label>
    <input type="date" id="date">
  </div>

  <div class="theme-selector">
    <label for="theme">Theme:</label>
    <select id="theme">
      <option value="default">Default</option>
      <option value="pastel-blue">Pastel Blue</option>
      <option value="pastel-pink">Pastel Pink</option>
      <option value="pastel-green">Pastel Green</option>
      <option value="pastel-purple">Pastel Purple</option>
      <option value="pastel-yellow">Pastel Yellow</option>
      <option value="pastel-mint">Pastel Mint</option>
      <option value="pastel-orange">Pastel Orange</option>
      <option value="amoled">AMOLED Dark</option>
    </select>
  </div>

  <div class="input-container">
    <input type="number" id="inputMinutes" placeholder="Minutes" min="0" />
  </div>

  <div class="time" id="time">00:00:00</div>

  <div class="buttons">
    <button id="startStop">Start</button>
    <button id="reset">Reset</button>
  </div>

  <div class="doodles">
    <img src="https://img.icons8.com/external-flatart-icons-lineal-color-flatarticons/64/null/external-books-back-to-school-flatart-icons-lineal-color-flatarticons.png" alt="Books" style="top: 10%; left: 15%;">
    <img src="https://img.icons8.com/external-flatart-icons-lineal-color-flatarticons/64/null/external-light-bulb-education-flatart-icons-lineal-color-flatarticons.png" alt="Light Bulb" style="top: 30%; left: 60%;">
    <img src="https://img.icons8.com/external-flatart-icons-lineal-color-flatarticons/64/null/external-laptop-education-flatart-icons-lineal-color-flatarticons.png" alt="Laptop" style="top: 50%; left: 30%;">
    <img src="https://img.icons8.com/external-flatart-icons-lineal-color-flatarticons/64/null/external-pencil-education-flatart-icons-lineal-color-flatarticons.png" alt="Pencil" style="top: 70%; left: 10%;">
    <img src="https://img.icons8.com/external-flatart-icons-lineal-color-flatarticons/64/null/external-backpack-education-flatart-icons-lineal-color-flatarticons.png" alt="Backpack" style="top: 80%; left: 70%;">
  </div>

  <div class="usage-tracker" id="usageTracker">Total Study Time: 00:00:00</div>

  <script>
    let timerInterval;
    let isRunning = false;
    let totalSeconds = 0;
    let totalStudyTime = parseInt(localStorage.getItem('totalStudyTime')) || 0;

    const timeDisplay = document.getElementById('time');
    const startStopButton = document.getElementById('startStop');
    const resetButton = document.getElementById('reset');
    const inputMinutes = document.getElementById('inputMinutes');
    const themeSelector = document.getElementById('theme');
    const usageTracker = document.getElementById('usageTracker');

    function formatTime(seconds) {
      const hrs = String(Math.floor(seconds / 3600)).padStart(2, '0');
      const mins = String(Math.floor((seconds % 3600) / 60)).padStart(2, '0');
      const secs = String(seconds % 60).padStart(2, '0');
      return `${hrs}:${mins}:${secs}`;
    }

    function updateDisplay() {
      timeDisplay.textContent = formatTime(totalSeconds);
    }

    function updateUsageTracker() {
      usageTracker.textContent = `Total Study Time: ${formatTime(totalStudyTime)}`;
    }

    function startTimer() {
      timerInterval = setInterval(() => {
        if (totalSeconds > 0) {
          totalSeconds--;
          totalStudyTime++;
          updateDisplay();
          updateUsageTracker();
          localStorage.setItem('totalStudyTime', totalStudyTime);
        } else {
          stopTimer();
          alert('Time is up!');
        }
      }, 1000);
      isRunning = true;
      startStopButton.textContent = 'Stop';
    }

    function stopTimer() {
      clearInterval(timerInterval);
      isRunning = false;
      startStopButton.textContent = 'Start';
    }

    startStopButton.addEventListener('click', () => {
      if (!isRunning) {
        const minutes = parseInt(inputMinutes.value) || 0;
        if (totalSeconds === 0 && minutes > 0) {
          totalSeconds = minutes * 60;
          updateDisplay();
        }
        if (totalSeconds > 0) {
          startTimer();
        }
      } else {
        stopTimer();
      }
    });

    resetButton.addEventListener('click', () => {
      stopTimer();
      totalSeconds = 0;
      updateDisplay();
    });

    themeSelector.addEventListener('change', () => {
      const theme = themeSelector.value;
      switch (theme) {
        case 'pastel-blue':
          document.documentElement.style.setProperty('--background-color', '#d0e8f2');
          document.documentElement.style.setProperty('--text-color', '#1e3d59');
          document.documentElement.style.setProperty('--button-bg', '#1e3d59');
          document.documentElement.style.setProperty('--button-hover-bg', '#4d648d');
          break;
        case 'pastel-pink':
          document.documentElement.style.setProperty('--background-color', '#f8d7da');
          document.documentElement.style.setProperty('--text-color', '#721c24');
          document.documentElement.style.setProperty('--button-bg', '#721c24');
          document.documentElement.style.setProperty('--button-hover-bg', '#a33e4b');
          break;
        case 'pastel-green':
          document.documentElement.style.setProperty('--background-color', '#d4edda');
          document.documentElement.style.setProperty('--text-color', '#155724');
          document.documentElement.style.setProperty('--button-bg', '#155724');
          document.documentElement.style.setProperty('--button-hover-bg', '#3e8c57');
          break;
        case 'pastel-purple':
          document.documentElement.style.setProperty('--background-color', '#e6e6fa');
          document.documentElement.style.setProperty('--text-color', '#4b0082');
          document.documentElement.style.setProperty('--button-bg', '#4b0082');
          document.documentElement.style.setProperty('--button-hover-bg', '#6a5acd');
          break;
        case 'pastel-yellow':
          document.documentElement.style.setProperty('--background-color', '#fffacd');
          document.documentElement.style.setProperty
