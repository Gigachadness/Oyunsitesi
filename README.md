<!DOCTYPE html>
<html lang="tr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Pong Oyunu</title>
    <style>
        body { margin: 0; padding: 0; overflow: hidden; }
        canvas { display: block; background: #000; }
        .controls { position: absolute; bottom: 20px; left: 50%; transform: translateX(-50%); display: flex; justify-content: space-around; width: 200px; }
        .button { background: #fff; border: none; padding: 10px; font-size: 1em; cursor: pointer; }
        .button:active { background: #ccc; }
    </style>
</head>
<body>
    <canvas id="pongCanvas"></canvas>
    <div class="controls">
        <button class="button" onclick="setGameMode('2player')">2 Oyunculu</button>
        <button class="button" onclick="setGameMode('ai')">AI Modu</button>
    </div>
    
    <script>
        const canvas = document.getElementById('pongCanvas');
        const ctx = canvas.getContext('2d');
        const paddleWidth = 10, paddleHeight = 100, ballSize = 10;
        let paddle1Y, paddle2Y, ballX, ballY, ballSpeedX, ballSpeedY, paddleSpeed = 5, gameMode, player1Up, player1Down, player2Up, player2Down;

        function setup() {
            canvas.width = window.innerWidth;
            canvas.height = window.innerHeight;
            paddle1Y = (canvas.height - paddleHeight) / 2;
            paddle2Y = (canvas.height - paddleHeight) / 2;
            ballX = canvas.width / 2;
            ballY = canvas.height / 2;
            ballSpeedX = 5 * (Math.random() > 0.5 ? 1 : -1);
            ballSpeedY = 3 * (Math.random() > 0.5 ? 1 : -1);
            gameMode = '2player'; // Default to 2-player mode
            player1Up = false;
            player1Down = false;
            player2Up = false;
            player2Down = false;
            document.addEventListener('keydown', keyDownHandler);
            document.addEventListener('keyup', keyUpHandler);
        }

        function draw() {
            ctx.clearRect(0, 0, canvas.width, canvas.height);

            ctx.fillStyle = '#fff';
            ctx.fillRect(0, paddle1Y, paddleWidth, paddleHeight); // Player 1 Paddle
            ctx.fillRect(canvas.width - paddleWidth, paddle2Y, paddleWidth, paddleHeight); // Player 2 Paddle

            ctx.beginPath();
            ctx.arc(ballX, ballY, ballSize, 0, Math.PI * 2);
            ctx.fillStyle = '#fff';
            ctx.fill();
            ctx.closePath();
        }

        function update() {
            // Ball movement
            ballX += ballSpeedX;
            ballY += ballSpeedY;

            if (ballY + ballSize > canvas.height || ballY - ballSize < 0) {
                ballSpeedY = -ballSpeedY;
            }

            // Paddle collisions
            if (ballX - ballSize < paddleWidth && ballY > paddle1Y && ballY < paddle1Y + paddleHeight) {
                ballSpeedX = -ballSpeedX;
                ballX = paddleWidth + ballSize;
            }

            if (ballX + ballSize > canvas.width - paddleWidth && ballY > paddle2Y && ballY < paddle2Y + paddleHeight) {
                ballSpeedX = -ballSpeedX;
                ballX = canvas.width - paddleWidth - ballSize;
            }

            // AI mode
            if (gameMode === 'ai') {
                if (ballY > paddle2Y + paddleHeight / 2) {
                    paddle2Y += paddleSpeed;
                } else {
                    paddle2Y -= paddleSpeed;
                }
            }

            // Player movement
            if (player1Up && paddle1Y > 0) {
                paddle1Y -= paddleSpeed;
            }
            if (player1Down && paddle1Y + paddleHeight < canvas.height) {
                paddle1Y += paddleSpeed;
            }
            if (gameMode === '2player') {
                if (player2Up && paddle2Y > 0) {
                    paddle2Y -= paddleSpeed;
                }
                if (player2Down && paddle2Y + paddleHeight < canvas.height) {
                    paddle2Y += paddleSpeed;
                }
            }
        }

        function drawGame() {
            draw();
            update();
            requestAnimationFrame(drawGame);
        }

        function keyDownHandler(event) {
            switch (event.key) {
                case 'ArrowUp':
                    if (gameMode === '2player') {
                        player2Up = true;
                    }
                    break;
                case 'ArrowDown':
                    if (gameMode === '2player') {
                        player2Down = true;
                    }
                    break;
                case 'w':
                    player1Up = true;
                    break;
                case 's':
                    player1Down = true;
                    break;
            }
        }

        function keyUpHandler(event) {
            switch (event.key) {
                case 'ArrowUp':
                    player2Up = false;
                    break;
                case 'ArrowDown':
                    player2Down = false;
                    break;
                case 'w':
                    player1Up = false;
                    break;
                case 's':
                    player1Down = false;
                    break;
            }
        }

        function setGameMode(mode) {
            gameMode = mode;
            setup();
            drawGame();
        }

        setup();
        drawGame();
    </script>
</body>
</html>
