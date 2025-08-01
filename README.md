<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Snake Game 🐍</title>
  <style>
    body {
      background: #111;
      color: #fff;
      font-family: Arial, sans-serif;
      text-align: center;
      margin: 0;
    }
    canvas {
      background: #222;
      display: block;
      margin: 20px auto;
      border: 2px solid #555;
    }
    #score {
      font-size: 1.3em;
    }
    #controls {
      margin: 20px auto;
      display: flex;
      justify-content: center;
      gap: 10px;
    }
    button {
      background: #333;
      color: #0f0;
      font-size: 1.1em;
      padding: 10px 20px;
      border: none;
      border-radius: 5px;
    }
    button:hover {
      background: #555;
    }
  </style>
</head>
<body>
  <h1>🐍 Snake Game</h1>
  <div id="score">Score: 0</div>
  <canvas id="game" width="400" height="400" tabindex="1"></canvas>

  <!-- Optional Touch Controls -->
  <div id="controls">
    <button onclick="turn('up')">⬆️</button>
    <button onclick="turn('left')">⬅️</button>
    <button onclick="turn('down')">⬇️</button>
    <button onclick="turn('right')">➡️</button>
  </div>

  <script>
    const canvas = document.getElementById("game");
    const ctx = canvas.getContext("2d");
    canvas.focus();

    const tileSize = 20;
    const tileCount = canvas.width / tileSize;
    let snake = [{ x: 10, y: 10 }];
    let direction = { x: 1, y: 0 };
    let nextDirection = { x: 1, y: 0 };
    let food = randomPosition();
    let score = 0;

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

    function gameLoop() {
      direction = nextDirection;

      const head = {
        x: snake[0].x + direction.x,
        y: snake[0].y + direction.y
      };

      if (
        head.x < 0 || head.y < 0 ||
        head.x >= tileCount || head.y >= tileCount ||
        snake.some(s => s.x === head.x && s.y === head.y)
      ) {
        alert("Game over! Final score: " + score);
        location.reload();
        return;
      }

      snake.unshift(head);

      if (head.x === food.x && head.y === food.y) {
        score += 10;
        document.getElementById("score").textContent = "Score: " + score;
        food = randomPosition();
      } else {
        snake.pop();
      }

      draw();
    }

    function draw() {
      ctx.fillStyle = "#222";
      ctx.fillRect(0, 0, canvas.width, canvas.height);

      ctx.fillStyle = "#0f0";
      for (let s of snake) {
        ctx.fillRect(s.x * tileSize, s.y * tileSize, tileSize - 2, tileSize - 2);
      }

      ctx.fillStyle = "#f00";
      ctx.fillRect(food.x * tileSize, food.y * tileSize, tileSize - 2, tileSize - 2);
    }

    setInterval(gameLoop, 100);
  </script>
</body>
</html>
