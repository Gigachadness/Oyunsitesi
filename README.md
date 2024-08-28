<!DOCTYPE html>
<html lang="tr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Pong Oyunu - 2 Oyunculu</title>
    <style>
        body { margin: 0; padding: 0; background: #000; overflow: hidden; display: flex; flex-direction: column; align-items: center; justify-content: center; height: 100vh; }
        canvas { background: #222; display: block; margin: 0 auto; border: 2px solid #fff; }
        .controls { position: absolute; bottom: 10px; width: 100%; display: flex; justify-content: space-between; }
        .controls div { display: flex; flex-direction: column; align-items: center; }
        .button { background: #fff; border: 2px solid #000; padding: 15px; margin: 5px; font-size: 1.2em; cursor: pointer; }
        .button:active { background: #ccc; }
    </style>
</head>
<body>
    <canvas id="pongCanvas"></canvas>
    <div class="controls">
        <div>
            <button class="button" onclick="movePaddle1('up')">1P ↑</button>
            <button class="button" onclick="movePaddle1('down')">1P ↓</button>
        </div>
        <div>
            <button class="button" onclick="movePaddle2('up')">2P ↑</button>
            <button class="button" onclick="movePaddle2('down')">2P ↓</button>
        </div>
    </div>

    <script>
        const canvas = document.getElementById('pongCanvas');
        const ctx = canvas.getContext('2d');
        const paddleWidth = 10, paddleHeight = 100;
        let width, height, ballX, ballY, ballSpeedX, ballSpeedY, paddle1Y, paddle2Y;
        const paddleSpeed = 10;

        function setup() {
            width = window.innerWidth * 0.9;
            height = window.innerHeight * 0.6;
            canvas.width = width;
            canvas.height = height;
            paddle1Y = paddle2Y = (height - paddleHeight) / 2;
            ballReset();
            setInterval(gameLoop, 1000 / 60);
        }

        function ballReset() {
            ballX = width / 2;
            ballY = height / 2;
            ballSpeedX = Math.random() > 0.5 ? 5 : -5;
            ballSpeedY = Math.random() > 0.5 ? 5 : -5;
        }

        function gameLoop() {
            moveBall();
            drawEverything();
        }

        function moveBall() {
            ballX += ballSpeedX;
            ballY += ballSpeedY;

            if (ballY <= 0 || ballY >= height) ballSpeedY = -ballSpeedY;

            if (ballX <= paddleWidth) {
                if (ballY > paddle1Y && ballY < paddle1Y + paddleHeight) {
                    ballSpeedX = -ballSpeedX;
                } else if (ballX <= 0) {
                    ballReset();
                }
            }

            if (ballX >= width - paddleWidth) {
                if (ballY > paddle2Y && ballY < paddle2Y + paddleHeight) {
                    ballSpeedX = -ballSpeedX;
                } else if (ballX >= width) {
                    ballReset();
                }
            }
        }

        function drawEverything() {
            ctx.clearRect(0, 0, width, height);
            ctx.fillStyle = '#fff';

            // Sol paddle (1. oyuncu)
            ctx.fillRect(0, paddle1Y, paddleWidth, paddleHeight);

            // Sağ paddle (2. oyuncu)
            ctx.fillRect(width - paddleWidth, paddle2Y, paddleWidth, paddleHeight);

            // Top
            ctx.beginPath();
            ctx.arc(ballX, ballY, 10, 0, Math.PI * 2);
            ctx.fill();
        }

        function movePaddle1(direction) {
            if (direction === 'up') {
                paddle1Y = Math.max(0, paddle1Y - paddleSpeed);
            } else {
                paddle1Y = Math.min(height - paddleHeight, paddle1Y + paddleSpeed);
            }
        }

        function movePaddle2(direction) {
            if (direction === 'up') {
                paddle2Y = Math.max(0, paddle2Y - paddleSpeed);
            } else {
                paddle2Y = Math.min(height - paddleHeight, paddle2Y + paddleSpeed);
            }
        }

        setup();
    </script>
</body>
</html>
