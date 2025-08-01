<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
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
      display: block;
      margin: 20px auto;
      background: #222;
      border: 2px solid #555;
    }
    #score {
      font-size: 1.4em;
      margin-top: 10px;
    }
  </style>
</head>
<body>
  <h1>🐍 Snake Game</h1>
  <div id="score">Score: 0</div>
  <canvas id="game" width="400" height="400"></canvas>
  <script>
    const canvas = document.getElementById("game");
    const ctx = canvas.getContext("2d");

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

    document.addEventListener("keydown", (e) => {
      if (e.key === "ArrowUp" && direction.y !== 1) nextDirection = { x: 0, y: -1 };
      if (e.key === "ArrowDown" && direction.y !== -1) nextDirection = { x: 0, y: 1 };
      if (e.key === "ArrowLeft" && direction.x !== 1) nextDirection = { x: -1, y: 0 };
      if (e.key === "ArrowRight" && direction.x !== -1) nextDirection = { x: 1, y: 0 };
    });

    function gameLoop() {
      direction = nextDirection;

      const head = {
        x: snake[0].x + direction.x,
        y: snake[0].y + direction.y
      };

      // Game over conditions
      if (
        head.x < 0 || head.y < 0 ||
        head.x >= tileCount || head.y >= tileCount ||
        snake.some(seg => seg.x === head.x && seg.y === head.y)
      ) {
        alert("💀 Game Over! Final Score: " + score);
        location.reload();
        return;
      }

      snake.unshift(head);

      // Eat food
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

      // Draw snake
      ctx.fillStyle = "#0f0";
      for (let s of snake) {
        ctx.fillRect(s.x * tileSize, s.y * tileSize, tileSize - 2, tileSize - 2);
      }

      // Draw food
      ctx.fillStyle = "#f00";
      ctx.fillRect(food.x * tileSize, food.y * tileSize, tileSize - 2, tileSize - 2);
    }

    setInterval(gameLoop, 100);
  </script>
</body>
</html>
