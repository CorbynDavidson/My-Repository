<script>
  const canvas = document.getElementById("game");
  const ctx = canvas.getContext("2d");
  const scoreDisplay = document.getElementById("score");
  const startBtn = document.getElementById("startBtn");
  const controls = document.getElementById("controls");

  const tileSize = 20;
  const tileCount = canvas.width / tileSize;

  let snake, direction, nextDirection, food, score, gameRunning;
  let lastUpdateTime = 0;
  let interval = 160; // slow it down a bit

  function initGame() {
    snake = [{ x: 10, y: 10 }];
    direction = { x: 1, y: 0 };
    nextDirection = { x: 1, y: 0 };
    food = randomPosition();
    score = 0;
    scoreDisplay.textContent = "Score: 0";
    gameRunning = true;
    lastUpdateTime = 0;
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

  function gameLoop(currentTime) {
    if (!gameRunning) return;

    if (currentTime - lastUpdateTime > interval) {
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
        alert("💀 Game over! Final score: " + score);
        startBtn.style.display = "inline-block";
        gameRunning = false;
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
      lastUpdateTime = currentTime;
    }

    requestAnimationFrame(gameLoop);
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

  startBtn.addEventListener("click", () => {
    initGame();
    canvas.style.display = "block";
    controls.style.display = "grid";
    startBtn.style.display = "none";
    canvas.focus();
  });
</script>