<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Snake GameBoy D-Pad 🐍</title>
  <style>
    body {
      background: #1a1a1a;
      font-family: monospace;
      color: #0f0;
      margin: 0;
      display: flex;
      flex-direction: column;
      align-items: center;
    }
    h1 {
      margin: 20px;
      color: #0f0;
    }
    canvas {
      background: #222;
      border: 5px solid #0f0;
    }
    #score {
      margin-top: 10px;
      font-size: 1.2em;
    }
    #dpad {
      display: grid;
      grid-template-areas:
        ". up ."
        "left center right"
        ". down .";
      grid-gap: 5px;
      margin: 20px;
    }
    #dpad button {
      width: 50px;
      height: 50px;
      background-color: #0f0;
      border: none;
      font-size: 1.5em;
      color: #000;
      border-radius: 5px;
    }
    #startBtn {
      background-color: #0f0;
      color: #000;
      font-size: 1.2em;
      padding: 10px 20px;
      border: none;
      border-radius: 8px;
      margin-top: 10px;
    }
  </style>
</head>
<body>
  <h1>🐍 Snake - GameBoy D-Pad</h1>
  <div id="score">Score: 0</div>
  <button id="startBtn">▶️ Start Game</button>
  <canvas id="game" width="400" height="400" style="display:none;" tabindex="1"></canvas>
  <div id="dpad" style="display:none;">
    <button style="grid-area: up;" onclick="turn('up')">▲</button>
    <button style="grid-area: left;" onclick="turn('left')">◀</button>
    <div style="grid-area: center;"></div>
    <button style="grid-area: right;" onclick="turn('right')">▶</button>
    <button style="grid-area: down;" onclick="turn('down')">▼</button>
  </div>

  <script>
    const canvas = document.getElementById("game");
    const ctx = canvas.getContext("2d");
    const scoreDisplay = document.getElementById("score");
    const startBtn = document.getElementById("startBtn");
    const dpad = document.getElementById("dpad");

    const tileSize = 20;
    const tileCount = canvas.width / tileSize;
    let snake, direction, nextDirection, food, score, gameRunning, lastMoveTime;

    function initGame() {
      snake = [{ x: 10, y: 10 }];
      direction = { x: 1, y: 0 };
      nextDirection = { x: 1, y: 0 };
      food = randomPosition();
      score = 0;
      gameRunning = true;
      lastMoveTime = 0;
      scoreDisplay.textContent = "Score: 0";
      requestAnimationFrame(gameLoop);
    }

    function randomPosition() {
      return {
        x: Math.floor(Math.random() * tileCount),
        y: Math.floor(Math.random() * tileCount)
      };
    }

    function turn(dir) {
      if (dir === 'up' && direction.y !== 1) nextDirection = { x: 0, y: -1 };
      if (dir === 'down' && direction.y !== -1) nextDirection = { x: 0, y: 1 };
      if (dir === 'left' && direction.x !== 1) nextDirection = { x: -1, y: 0 };
      if (dir === 'right' && direction.x !== -1) nextDirection = { x: 1, y: 0 };
    }

    document.addEventListener("keydown", (e) => {
      if (e.key === "ArrowUp") turn("up");
      if (e.key === "ArrowDown") turn("down");
      if (e.key === "ArrowLeft") turn("left");
      if (e.key === "ArrowRight") turn("right");
    });

    function gameLoop(timestamp) {
      if (!gameRunning) return;

      if (timestamp - lastMoveTime > 250) { // slower speed for easier play
        direction = nextDirection;
        const head = { x: snake[0].x + direction.x, y: snake[0].y + direction.y };

        if (
          head.x < 0 || head.y < 0 ||
          head.x >= tileCount || head.y >= tileCount ||
          snake.some(s => s.x === head.x && s.y === head.y)
        ) {
          alert("💀 Game over! Final score: " + score);
          gameRunning = false;
          startBtn.style.display = "inline-block";
          return;
        }

        snake.unshift(head);

        if (head.x === food.x && head.y === food.y) {
          score += 10;
          scoreDisplay.textContent = "Score: " + score;
          food = randomPosition();
        } else {
          snake.pop();
        }

        draw();
        lastMoveTime = timestamp;
      }

      requestAnimationFrame(gameLoop);
    }

    function draw() {
      ctx.fillStyle = "#222";
      ctx.fillRect(0, 0, canvas.width, canvas.height);

      ctx.fillStyle = "#0f0";
      snake.forEach(s => {
        ctx.fillRect(s.x * tileSize, s.y * tileSize, tileSize - 2, tileSize - 2);
      });

      ctx.fillStyle = "#f00";
      ctx.fillRect(food.x * tileSize, food.y * tileSize, tileSize - 2, tileSize - 2);
    }

    startBtn.addEventListener("click", () => {
      initGame();
      canvas.style.display = "block";
      dpad.style.display = "grid";
      startBtn.style.display = "none";
      canvas.focus();
    });
  </script>
</body>
</html>