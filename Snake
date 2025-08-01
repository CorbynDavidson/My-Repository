<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Snake Game 🐍</title>
  <style>
    body {
      margin: 0;
      background: #111;
      color: #fff;
      font-family: Arial, sans-serif;
      display: flex;
      flex-direction: column;
      align-items: center;
    }
    h1 {
      margin-top: 20px;
    }
    canvas {
      background: #222;
      display: block;
      margin: 20px auto;
    }
    #score {
      font-size: 1.2em;
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

    const gridSize = 20;
    const tileCount = canvas.width / gridSize;
    let snake = [{ x: 10, y: 10 }];
    let food = { x: 5, y: 5 };
    let dx = 0;
    let dy = 0;
    let score = 0;

    document.addEventListener("keydown", e => {
      if (e.key === "ArrowUp" && dy === 0) { dx = 0; dy = -1; }
      else if (e.key === "ArrowDown" && dy === 0) { dx = 0; dy = 1; }
      else if (e.key === "ArrowLeft" && dx === 0) { dx = -1; dy = 0; }
      else if (e.key === "ArrowRight" && dx === 0) { dx = 1; dy = 0; }
    });

    function gameLoop() {
      const head = { x: snake[0].x + dx, y: snake[0].y + dy };

      if (head.x < 0 || head.y < 0 || head.x >= tileCount || head.y >= tileCount || snake.some(s => s.x === head.x && s.y === head.y)) {
        alert("Game over! Your score: " + score);
        location.reload();
        return;
      }

      snake.unshift(head);

      if (head.x === food.x && head.y === food.y) {
        score += 10;
        document.getElementById("score").textContent = "Score: " + score;
        food = {
          x: Math.floor(Math.random() * tileCount),
          y: Math.floor(Math.random() * tileCount)
        };
      } else {
        snake.pop();
      }

      ctx.fillStyle = "#222";
      ctx.fillRect(0, 0, canvas.width, canvas.height);

      ctx.fillStyle = "#0f0";
      snake.forEach(s => ctx.fillRect(s.x * gridSize, s.y * gridSize, gridSize - 2, gridSize - 2));

      ctx.fillStyle = "#f00";
      ctx.fillRect(food.x * gridSize, food.y * gridSize, gridSize - 2, gridSize - 2);

      setTimeout(gameLoop, 100);
    }

    gameLoop();
  </script>
</body>
</html>
