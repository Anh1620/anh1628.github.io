<!DOCTYPE html>
<html lang="vi">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>CMSN</title>
  <style>
    body {
      margin: 0;
      height: 100vh;
      display: flex;
      justify-content: center;
      align-items: center;
      background: url("a.jpg") no-repeat center center fixed;
      background-size: cover;
      font-family: Arial, sans-serif;
      overflow: hidden;
    }

    .envelope {
      width: 300px;
      height: 200px;
      background: #ee88dd;
      position: relative;
      cursor: pointer;
      box-shadow: 0 8px 16px rgba(0, 0, 0, 0.2);
      transition: transform 0.3s;
      border: 2px solid #ce68b4;
    }

    .envelope:hover { transform: scale(1.05); }

    .envelope .flap {
      width: 100%;
      height: 100px;
      background: #ee88dd;
      clip-path: polygon(0 0, 50% 100%, 100% 0);
      position: absolute;
      top: 0;
      transition: transform 0.5s;
      transform-origin: top;
      border-top: 2px solid #ce68b4;
      border-left: 2px solid #ce68b4;
      border-right: 2px solid #ce68b4;
    }

    .envelope.open .flap { transform: rotateX(180deg); }

    .letter {
      display: none;
      position: fixed;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
      background: rgba(255, 255, 255, 0.95);
      justify-content: center;
      align-items: center;
      text-align: center;
      animation: fadeIn 1s;
    }

    .letter.show { display: flex; }

    .letter-content {
      max-width: 600px;
      padding: 20px;
      color: #333;
      font-size: 1.2em;
      position: relative;
    }

    .letter-content h1 {
      color: #e91e63;
      font-family: 'Dancing Script', cursive;
    }

    .greeting {
      font-size: 1.2em;
      white-space: pre-line; /* giữ xuống dòng */
      margin-top: 20px;
      color: #444;
    }

    .close-btn {
      position: absolute;
      top: 20px;
      right: 20px;
      font-size: 1.5em;
      cursor: pointer;
      color: #333;
    }

    @keyframes fadeIn { from { opacity: 0; } to { opacity: 1; } }

    .confetti-piece {
      position: fixed;
      top: -10vh;
      width: 10px;
      height: 10px;
      background-color: transparent;
      border-radius: 50%;
      opacity: 0;
      animation: fall 10s linear forwards;
      pointer-events: none;
    }

    @keyframes fall {
      0% { transform: translateY(0) rotate(0deg) scale(0.5); opacity: 0.8; }
      100% { transform: translateY(120vh) rotate(720deg) scale(1.2); opacity: 0; }
    }

    .open-text {
      position: absolute;
      top: 50%;
      left: 50%;
      transform: translate(-50%, -50%);
      color: #fff;
      font-weight: bold;
      font-size: 1.2em;
      text-shadow: 1px 1px 2px rgba(0, 0, 0, 0.5);
      z-index: 10;
      transition: opacity 0.3s;
    }
    .envelope.open .open-text { opacity: 0; }
  </style>
</head>
<body>
  <div class="envelope" id="envelope">
    <div class="flap"></div>
    <div class="open-text">Nhấn vào</div>
  </div>

  <div class="letter" id="letter">
    <div class="close-btn" id="closeLetter">✖</div>
    <div class="letter-content">
      <h1>HAPPY BIRTHDAY</h1>
      <p class="greeting" id="greetingText"></p>
    </div>
  </div>

  <audio id="birthdaySong" loop>
    <source src="p.mp3" type="audio/mp3">
  </audio>

  <script>
    const envelope = document.getElementById('envelope');
    const letter = document.getElementById('letter');
    const closeLetter = document.getElementById('closeLetter');
    const birthdaySong = document.getElementById('birthdaySong');
    const greetingEl = document.getElementById('greetingText');
    let confettiInterval;

    const greetingMessage = `Chúc bạn sinh nhật thật vui vẻ 🎉
Mọi điều tốt đẹp sẽ đến với bạn ✨
Luôn hạnh phúc và thành công 🎂🎁`;

    function typeWriter(text, element, speed = 70) {
      let i = 0;
      element.textContent = "";
      const interval = setInterval(() => {
        element.textContent += text.charAt(i);
        i++;
        if (i >= text.length) clearInterval(interval);
      }, speed);
    }

    function createConfetti() {
      const colors = ['#ff6b6b', '#f9ca24', '#78e08f', '#48dbfb', '#c0392b', '#ff7979', '#ffb142', '#a6d96a'];
      for (let i = 0; i < 200; i++) {
        const confetti = document.createElement('div');
        confetti.classList.add('confetti-piece');
        confetti.style.left = `${Math.random() * 100}vw`;
        confetti.style.animationDuration = `${Math.random() * 5 + 5}s`;
        confetti.style.backgroundColor = colors[Math.floor(Math.random() * colors.length)];
        document.body.appendChild(confetti);
      }
    }

    envelope.addEventListener('click', () => {
      envelope.classList.add('open');
      setTimeout(() => {
        letter.classList.add('show');
        birthdaySong.play();
        typeWriter(greetingMessage, greetingEl, 60);
        createConfetti();
        confettiInterval = setInterval(createConfetti, 5000);
      }, 500);
    });

    closeLetter.addEventListener('click', () => {
      letter.classList.remove('show');
      envelope.classList.remove('open');
      birthdaySong.pause();
      birthdaySong.currentTime = 0;
      greetingEl.textContent = "";
      clearInterval(confettiInterval);
      const confettiPieces = document.querySelectorAll('.confetti-piece');
      confettiPieces.forEach(c => c.remove());
    });
  </script>

  <link href="https://fonts.googleapis.com/css2?family=Dancing+Script:wght@700&display=swap" rel="stylesheet">
</body>
</html>
