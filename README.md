# birthday-puzzel
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>A Special Birthday Surprise! 🎂</title>
  <style>
    :root {
      --primary: #ff4d6d;
      --primary-dark: #c9184a;
      --accent: #ffb3c1;
      --bg-start: #0f172a;
      --bg-end: #1e1b4b;
      --card-bg: rgba(255, 255, 255, 0.08);
    }

    * {
      box-sizing: border-box;
      margin: 0;
      padding: 0;
      font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
    }

    body {
      min-height: 100vh;
      background: linear-gradient(135deg, var(--bg-start), var(--bg-end));
      color: #fff;
      display: flex;
      justify-content: center;
      align-items: center;
      padding: 20px;
      overflow-x: hidden;
      position: relative;
    }

    .container {
      width: 100%;
      max-width: 500px;
      background: var(--card-bg);
      backdrop-filter: blur(16px);
      border: 1px solid rgba(255, 255, 255, 0.15);
      border-radius: 24px;
      padding: 32px 24px;
      box-shadow: 0 20px 50px rgba(0, 0, 0, 0.4);
      text-align: center;
      position: relative;
      z-index: 10;
      transition: all 0.4s ease;
    }

    h1, h2 {
      font-size: 1.8rem;
      margin-bottom: 16px;
      background: linear-gradient(45deg, #ff758c, #ff7eb3);
      -webkit-background-clip: text;
      -webkit-text-fill-color: transparent;
    }

    p {
      color: #cbd5e1;
      line-height: 1.6;
      font-size: 1.05rem;
      margin-bottom: 24px;
    }

    .btn-group {
      display: flex;
      justify-content: center;
      align-items: center;
      gap: 16px;
      margin-top: 20px;
      position: relative;
      min-height: 80px;
    }

    .btn {
      padding: 14px 28px;
      border: none;
      border-radius: 50px;
      font-weight: 600;
      font-size: 1rem;
      cursor: pointer;
      transition: transform 0.2s, box-shadow 0.2s;
      outline: none;
    }

    .btn-primary {
      background: linear-gradient(45deg, var(--primary), var(--primary-dark));
      color: #fff;
      box-shadow: 0 8px 20px rgba(255, 77, 109, 0.4);
    }

    .btn-primary:hover {
      transform: translateY(-2px);
      box-shadow: 0 12px 24px rgba(255, 77, 109, 0.6);
    }

    .btn-secondary {
      background: rgba(255, 255, 255, 0.1);
      color: #e2e8f0;
      border: 1px solid rgba(255, 255, 255, 0.2);
      position: relative;
      z-index: 5;
    }

    textarea {
      width: 100%;
      height: 110px;
      background: rgba(0, 0, 0, 0.2);
      border: 1px solid rgba(255, 255, 255, 0.2);
      border-radius: 16px;
      padding: 14px;
      color: #fff;
      font-size: 1rem;
      resize: none;
      margin-bottom: 20px;
      outline: none;
    }

    textarea:focus {
      border-color: var(--primary);
    }

    .hidden {
      display: none;
    }

    .fade-in {
      animation: fadeIn 0.6s ease forwards;
    }

    @keyframes fadeIn {
      from { opacity: 0; transform: translateY(15px); }
      to { opacity: 1; transform: translateY(0); }
    }

    .letter {
      background: rgba(255, 255, 255, 0.05);
      border-left: 4px solid var(--primary);
      padding: 20px;
      border-radius: 12px;
      text-align: left;
      margin: 20px 0;
      font-size: 1.05rem;
      line-height: 1.7;
      color: #f1f5f9;
    }

    .quote-box {
      font-style: italic;
      color: var(--accent);
      background: rgba(255, 255, 255, 0.05);
      padding: 12px;
      border-radius: 10px;
      margin-bottom: 16px;
    }

    canvas {
      position: fixed;
      top: 0;
      left: 0;
      width: 100vw;
      height: 100vh;
      pointer-events: none;
      z-index: 100;
    }
  </style>
</head>
<body>

  <canvas id="glitterCanvas"></canvas>

  <div class="container">
    <!-- STEP 1: Puzzle Question -->
    <div id="step1" class="fade-in">
      <h1>Security Question 🔒</h1>
      <p>Before you unlock your birthday surprise, verify your identity!<br><strong>Who is the person sending you this puzzle?</strong></p>
      
      <div class="btn-group" id="btnContainer">
        <button class="btn btn-primary" onclick="goToStep(2)">Your Special Person ❤️</button>
        <button class="btn btn-secondary" id="runawayBtn">Just Your Friend</button>
      </div>
    </div>

    <!-- STEP 2: Thought Prompt -->
    <div id="step2" class="hidden">
      <h1>One Quick Question... ✍️</h1>
      <p>Almost there! In a few words, what do you think about the sender?</p>
      <textarea id="userInput" placeholder="Write whatever comes to your mind..."></textarea>
      <button class="btn btn-primary" onclick="unlockFinalStep()">Unlock Birthday Surprise 🎉</button>
    </div>

    <!-- STEP 3: Final Birthday Message -->
    <div id="step3" class="hidden">
      <h1>Happy Birthday! 🎂✨</h1>
      
      <div id="userThoughtDisplay" class="quote-box"></div>

      <div class="letter">
        <p>Happy Birthday to an truly amazing person! ❤️</p>
        <br>
        <p>We’ve been through our fair share of ups and downs, but no matter what, we just keep getting closer. Things might be complicated and unofficial right now, but I want you to know that the feelings I have for you have never died.</p>
        <br>
        <p>You are so sweet, kind, and genuinely incredible. I appreciate you more than words can say, and I love you. I hope this year brings you all the happiness in the world!</p>
      </div>

      <p style="font-size: 1.3rem; margin-top: 15px;">🎉 Enjoy your special day! 🎉</p>
    </div>
  </div>

  <script>
    // Runaway Button Logic
    const runawayBtn = document.getElementById('runawayBtn');
    const container = document.querySelector('.container');

    function moveButton() {
      const containerRect = container.getBoundingClientRect();
      const btnRect = runawayBtn.getBoundingClientRect();

      const maxX = containerRect.width - btnRect.width - 40;
      const maxY = containerRect.height - btnRect.height - 40;

      const randomX = Math.floor(Math.random() * maxX) - (maxX / 2);
      const randomY = Math.floor(Math.random() * maxY) - (maxY / 2);

      runawayBtn.style.transform = `translate(${randomX}px, ${randomY}px)`;
    }

    runawayBtn.addEventListener('mouseover', moveButton);
    runawayBtn.addEventListener('touchstart', function(e) {
      e.preventDefault();
      moveButton();
    });

    // Step Switching
    function goToStep(stepNum) {
      document.getElementById('step1').classList.add('hidden');
      document.getElementById('step2').classList.remove('hidden');
      document.getElementById('step2').classList.add('fade-in');
    }

    function unlockFinalStep() {
      const input = document.getElementById('userInput').value.trim();
      const display = document.getElementById('userThoughtDisplay');

      if (input) {
        display.innerHTML = `"${input}"`;
      } else {
        display.style.display = 'none';
      }

      document.getElementById('step2').classList.add('hidden');
      document.getElementById('step3').classList.remove('hidden');
      document.getElementById('step3').classList.add('fade-in');

      triggerGlitters();
    }

    // Canvas Glitter Effects
    const canvas = document.getElementById('glitterCanvas');
    const ctx = canvas.getContext('2d');
    let particles = [];

    function resizeCanvas() {
      canvas.width = window.innerWidth;
      canvas.height = window.innerHeight;
    }
    window.addEventListener('resize', resizeCanvas);
    resizeCanvas();

    class Particle {
      constructor(x, y) {
        this.x = x;
        this.y = y;
        this.size = Math.random() * 6 + 2;
        this.speedX = (Math.random() - 0.5) * 8;
        this.speedY = (Math.random() - 0.5) * 8 - 3;
        this.color = `hsl(${Math.random() * 60 + 330}, 100%, 75%)`;
        this.alpha = 1;
        this.decay = Math.random() * 0.015 + 0.005;
      }

      update() {
        this.x += this.speedX;
        this.y += this.speedY;
        this.speedY += 0.1; // gravity
        this.alpha -= this.decay;
      }

      draw() {
        ctx.save();
        ctx.globalAlpha = Math.max(0, this.alpha);
        ctx.fillStyle = this.color;
        ctx.beginPath();
        ctx.arc(this.x, this.y, this.size, 0, Math.PI * 2);
        ctx.fill();
        ctx.restore();
      }
    }

    function triggerGlitters() {
      const centerX = window.innerWidth / 2;
      const centerY = window.innerHeight / 3;

      for (let i = 0; i < 120; i++) {
        particles.push(new Particle(centerX, centerY));
      }

      animateParticles();
    }

    function animateParticles() {
      ctx.clearRect(0, 0, canvas.width, canvas.height);

      for (let i = particles.length - 1; i >= 0; i--) {
        particles[i].update();
        particles[i].draw();

        if (particles[i].alpha <= 0) {
          particles.splice(i, 1);
        }
      }

      if (particles.length > 0) {
        requestAnimationFrame(animateParticles);
      }
    }
  </script>
</body>
</html>
