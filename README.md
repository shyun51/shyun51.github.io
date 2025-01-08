# shyun51.github.io
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Snake Game</title>
    <style>
        body {
            margin: 0;
            font-family: Arial, sans-serif;
            background-color: #333;
            color: #fff;
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
            overflow: hidden;
        }

        canvas {
            background-color: #444;
            border: 2px solid #888;
        }

        .score {
            position: absolute;
            top: 20px;
            left: 20px;
            font-size: 1.5rem;
            z-index: 10;
        }
    </style>
</head>
<body>
    <div class="score">Score: <span id="score">0</span></div>
    <canvas id="gameCanvas" width="400" height="400"></canvas>

    <script>
        const canvas = document.getElementById('gameCanvas');
        const ctx = canvas.getContext('2d');

        const box = 20; // Size of one grid box
        const canvasSize = canvas.width / box;

        let snake = [{ x: 10, y: 10 }]; // Initial position of the snake
        let food = { x: Math.floor(Math.random() * canvasSize), y: Math.floor(Math.random() * canvasSize) };
        let direction = 'RIGHT';
        let score = 0;

        function drawBox(x, y, color) {
            ctx.fillStyle = color;
            ctx.fillRect(x * box, y * box, box, box);
        }

        function drawSnake() {
            snake.forEach(segment => drawBox(segment.x, segment.y, 'lime'));
        }

        function drawFood() {
            drawBox(food.x, food.y, 'red');
        }

        function updateSnake() {
            const head = { ...snake[0] };
            if (direction === 'UP') head.y -= 1;
            if (direction === 'DOWN') head.y += 1;
            if (direction === 'LEFT') head.x -= 1;
            if (direction === 'RIGHT') head.x += 1;

            snake.unshift(head);

            // Check if snake eats food
            if (head.x === food.x && head.y === food.y) {
                score += 10;
                document.getElementById('score').textContent = score;
                food = { x: Math.floor(Math.random() * canvasSize), y: Math.floor(Math.random() * canvasSize) };
            } else {
                snake.pop();
            }
        }

        function checkCollision() {
            const [head, ...body] = snake;
            const hitWall = head.x < 0 || head.x >= canvasSize || head.y < 0 || head.y >= canvasSize;
            const hitSelf = body.some(segment => segment.x === head.x && segment.y === head.y);
            return hitWall || hitSelf;
        }

        function gameLoop() {
            if (checkCollision()) {
                alert('Game Over! Your score: ' + score);
                document.location.reload();
                return;
            }

            ctx.clearRect(0, 0, canvas.width, canvas.height);

            drawFood();
            updateSnake();
            drawSnake();
        }

        document.addEventListener('keydown', e => {
            if (e.key === 'ArrowUp' && direction !== 'DOWN') direction = 'UP';
            if (e.key === 'ArrowDown' && direction !== 'UP') direction = 'DOWN';
            if (e.key === 'ArrowLeft' && direction !== 'RIGHT') direction = 'LEFT';
            if (e.key === 'ArrowRight' && direction !== 'LEFT') direction = 'RIGHT';
        });

        setInterval(gameLoop, 100);
    </script>
</body>
</html>
