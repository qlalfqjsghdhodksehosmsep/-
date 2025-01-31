# -<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Snake Game</title>
    <style>
        body {
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
            background-color: #f0f0f0;
            margin: 0;
        }
        canvas {
            border: 2px solid black;
        }
    </style>
</head>
<body>
    <canvas id="snakeGame" width="600" height="400"></canvas>
    <script>
        const canvas = document.getElementById('snakeGame');
        const ctx = canvas.getContext('2d');

        const box = 10;
        let snake = [{ x: 5 * box, y: 5 * box }];
        let direction = 'RIGHT';
        let food = {
            x: Math.floor(Math.random() * (canvas.width / box)) * box,
            y: Math.floor(Math.random() * (canvas.height / box)) * box
        };
        let score = 0;

        document.addEventListener('keydown', directionControl);

        function directionControl(event) {
            if (event.key === 'ArrowLeft' && direction !== 'RIGHT') direction = 'LEFT';
            else if (event.key === 'ArrowUp' && direction !== 'DOWN') direction = 'UP';
            else if (event.key === 'ArrowRight' && direction !== 'LEFT') direction = 'RIGHT';
            else if (event.key === 'ArrowDown' && direction !== 'UP') direction = 'DOWN';
        }

        function collision(head, array) {
            return array.some(segment => head.x === segment.x && head.y === segment.y);
        }

        function draw() {
            ctx.clearRect(0, 0, canvas.width, canvas.height);

            // Draw food
            ctx.fillStyle = 'green';
            ctx.fillRect(food.x, food.y, box, box);

            // Draw snake
            for (let i = 0; i < snake.length; i++) {
                ctx.fillStyle = (i === 0) ? 'black' : 'lightgreen';
                ctx.fillRect(snake[i].x, snake[i].y, box, box);
                ctx.strokeStyle = 'darkgreen';
                ctx.strokeRect(snake[i].x, snake[i].y, box, box);
            }

            // Move snake
            let headX = snake[0].x;
            let headY = snake[0].y;

            if (direction === 'LEFT') headX -= box;
            if (direction === 'UP') headY -= box;
            if (direction === 'RIGHT') headX += box;
            if (direction === 'DOWN') headY += box;

            const newHead = { x: headX, y: headY };

            // Check for collision with food
            if (headX === food.x && headY === food.y) {
                score++;
                food = {
                    x: Math.floor(Math.random() * (canvas.width / box)) * box,
                    y: Math.floor(Math.random() * (canvas.height / box)) * box
                };
            } else {
                snake.pop(); // Remove the tail
            }

            // Check for collision with walls or self
            if (headX < 0 || headY < 0 || headX >= canvas.width || headY >= canvas.height || collision(newHead, snake)) {
                clearInterval(game);
                alert('Game Over! Your score: ' + score);
                document.location.reload();
            }

            snake.unshift(newHead);
        }

        const game = setInterval(draw, 100);
    </script>
</body>
</html>
