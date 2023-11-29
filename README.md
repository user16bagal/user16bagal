<!DOCTYPE html>
<html lang="es">
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
            margin: 0;
            background-color: #f0f0f0;
        }
        canvas {
            border: 1px solid #333;
        }
    </style>
</head>
<body>

<canvas id="snakeCanvas" width="400" height="400"></canvas>

<script>
    const canvas = document.getElementById("snakeCanvas");
    const ctx = canvas.getContext("2d");

    const boxSize = 20;
    let snake = [{ x: 10, y: 10 }];
    let food = { x: 15, y: 15 };
    let dx = 1;
    let dy = 0;

    function draw() {
        // Clear the canvas
        ctx.clearRect(0, 0, canvas.width, canvas.height);

        // Draw the snake
        ctx.fillStyle = "#4CAF50";
        snake.forEach(segment => drawRoundRect(segment.x * boxSize, segment.y * boxSize, boxSize, boxSize, 5));

        // Draw the food
        ctx.fillStyle = "#FF0000";
        drawCircle(food.x * boxSize + boxSize / 2, food.y * boxSize + boxSize / 2, boxSize / 2);
    }

    function drawRoundRect(x, y, width, height, radius) {
        ctx.beginPath();
        ctx.moveTo(x + radius, y);
        ctx.arcTo(x + width, y, x + width, y + height, radius);
        ctx.arcTo(x + width, y + height, x, y + height, radius);
        ctx.arcTo(x, y + height, x, y, radius);
        ctx.arcTo(x, y, x + width, y, radius);
        ctx.closePath();
        ctx.fill();
    }

    function drawCircle(x, y, radius) {
        ctx.beginPath();
        ctx.arc(x, y, radius, 0, 2 * Math.PI);
        ctx.closePath();
        ctx.fill();
    }

    function move() {
        const head = { x: snake[0].x + dx, y: snake[0].y + dy };
        snake.unshift(head);

        // Check if the snake eats the food
        if (head.x === food.x && head.y === food.y) {
            // Generate new food
            food = {
                x: Math.floor(Math.random() * (canvas.width / boxSize)),
                y: Math.floor(Math.random() * (canvas.height / boxSize))
            };
        } else {
            // Remove the last segment of the snake if it didn't eat the food
            snake.pop();
        }
    }

    function checkCollision() {
        const head = snake[0];

        // Check if the snake hits the walls or itself
        if (
            head.x < 0 || head.x >= canvas.width / boxSize ||
            head.y < 0 || head.y >= canvas.height / boxSize ||
            collisionWithItself(head)
        ) {
            alert("Game Over!");
            resetGame();
        }
    }

    function collisionWithItself(head) {
        // Check if the head collides with any other segment of the snake
        return snake.slice(1).some(segment => segment.x === head.x && segment.y === head.y);
    }

    function resetGame() {
        snake = [{ x: 10, y: 10 }];
        food = { x: 15, y: 15 };
        dx = 1;
        dy = 0;
    }

    function gameLoop() {
        move();
        checkCollision();
        draw();
    }

    document.addEventListener("keydown", event => {
        switch (event.key) {
            case "ArrowUp":
                if (dy !== 1) {
                    dx = 0;
                    dy = -1;
                }
                break;
            case "ArrowDown":
                if (dy !== -1) {
                    dx = 0;
                    dy = 1;
                }
                break;
            case "ArrowLeft":
                if (dx !== 1) {
                    dx = -1;
                    dy = 0;
                }
                break;
            case "ArrowRight":
                if (dx !== -1) {
                    dx = 1;
                    dy = 0;
                }
                break;
        }
    });

    setInterval(gameLoop, 100);

</script>

</body>
</html>
