<!DOCTYPE html>
<html lang="tr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Pong Oyunu</title>
    <style>
        body { margin: 0; padding: 0; display: flex; justify-content: center; align-items: center; background: #000; }
        canvas { border: 1px solid #fff; }
        #mobile-controls { position: absolute; bottom: 20px; width: 100%; text-align: center; }
        .control-button { width: 60px; height: 60px; font-size: 30px; margin: 10px; }
    </style>
</head>
<body>
    <canvas id="pongCanvas" width="640" height="480"></canvas>
    <div id="mobile-controls">
        <button class="control-button" id="up1">↑</button>
        <button class="control-button" id="down1">↓</button>
    </div>
    <script>
        const canvas = document.getElementById('pongCanvas');
        const ctx = canvas.getContext('2d');

        let paddleHeight = 75;
        let paddleWidth = 10;
        let paddleY1 = (canvas.height - paddleHeight) / 2;
        let paddleY2 = (canvas.height - paddleHeight) / 2;

        let ballRadius = 10;
        let x = canvas.width / 2;
        let y = canvas.height / 2;
        let dx = 2;
        let dy = -2;

        function drawPaddle(x, y) {
            ctx.beginPath();
            ctx.rect(x, y, paddleWidth, paddleHeight);
            ctx.fillStyle = "#fff";
            ctx.fill();
            ctx.closePath();
        }

        function drawBall() {
            ctx.beginPath();
            ctx.arc(x, y, ballRadius, 0, Math.PI * 2);
            ctx.fillStyle = "#fff";
            ctx.fill();
            ctx.closePath();
        }

        function draw() {
            ctx.clearRect(0, 0, canvas.width, canvas.height);
            drawPaddle(0, paddleY1);
            drawPaddle(canvas.width - paddleWidth, paddleY2);
            drawBall();

            if (y + dy < ballRadius || y + dy > canvas.height - ballRadius) {
                dy = -dy;
            }
            if (x + dx < ballRadius) {
                if (y > paddleY1 && y < paddleY1 + paddleHeight) {
                    dx = -dx;
                } else {
                    x = canvas.width / 2;
                    y = canvas.height / 2;
                    dx = -dx;
                }
            }
            if (x + dx > canvas.width - ballRadius) {
                if (y > paddleY2 && y < paddleY2 + paddleHeight) {
                    dx = -dx;
                } else {
                    x = canvas.width / 2;
                    y = canvas.height / 2;
                    dx = -dx;
                }
            }

            x += dx;
            y += dy;
        }

        let upPressed1 = false;
        let downPressed1 = false;

        document.addEventListener("keydown", function (event) {
            if (event.key === "ArrowUp") upPressed1 = true;
            if (event.key === "ArrowDown") downPressed1 = true;
        });

        document.addEventListener("keyup", function (event) {
            if (event.key === "ArrowUp") upPressed1 = false;
            if (event.key === "ArrowDown") downPressed1 = false;
        });

        document.getElementById('up1').addEventListener('touchstart', function () { upPressed1 = true; });
        document.getElementById('down1').addEventListener('touchstart', function () { downPressed1 = true; });
        document.getElementById('up1').addEventListener('touchend', function () { upPressed1 = false; });
        document.getElementById('down1').addEventListener('touchend', function () { downPressed1 = false; });

        function movePaddles() {
            if (upPressed1 && paddleY1 > 0) paddleY1 -= 5;
            if (downPressed1 && paddleY1 < canvas.height - paddleHeight) paddleY1 += 5;
            if (upPressed1 && paddleY2 > 0) paddleY2 -= 5;
            if (downPressed1 && paddleY2 < canvas.height - paddleHeight) paddleY2 += 5;
        }

        function gameLoop() {
            movePaddles();
            draw();
            requestAnimationFrame(gameLoop);
        }

        gameLoop();
    </script>
</body>
</html>
