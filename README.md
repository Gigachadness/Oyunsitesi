<!DOCTYPE html>
<html lang="tr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Pong Oyunu</title>
    <style>
        body { display: flex; justify-content: center; align-items: center; height: 100vh; background: #000; margin: 0; }
        canvas { background: #000; border: 1px solid #fff; }
    </style>
</head>
<body>
    <canvas id="pong" width="800" height="400"></canvas>
    <script>
        const canvas = document.getElementById('pong');
        const ctx = canvas.getContext('2d');

        const paddleWidth = 10, paddleHeight = 100, ballSize = 10;
        let player = { x: 10, y: canvas.height / 2 - paddleHeight / 2, width: paddleWidth, height: paddleHeight, dy: 0 };
        let ai = { x: canvas.width - paddleWidth - 10, y: canvas.height / 2 - paddleHeight / 2, width: paddleWidth, height: paddleHeight };
        let ball = { x: canvas.width / 2, y: canvas.height / 2, size: ballSize, dx: 4, dy: 4 };

        function draw() {
            ctx.clearRect(0, 0, canvas.width, canvas.height);
            ctx.fillStyle = '#fff';
            ctx.fillRect(player.x, player.y, player.width, player.height);
            ctx.fillRect(ai.x, ai.y, ai.width, ai.height);
            ctx.fillRect(ball.x, ball.y, ball.size, ball.size);
        }

        function update() {
            player.y += player.dy;
            if (player.y < 0) player.y = 0;
            if (player.y + player.height > canvas.height) player.y = canvas.height - player.height;

            ball.x += ball.dx;
            ball.y += ball.dy;

            if (ball.y < 0 || ball.y + ball.size > canvas.height) ball.dy *= -1;
            if (ball.x < player.x + player.width && ball.y > player.y && ball.y < player.y + player.height) ball.dx *= -1;
            if (ball.x + ball.size > ai.x && ball.y > ai.y && ball.y < ai.y + ai.height) ball.dx *= -1;

            if (ball.x < 0 || ball.x + ball.size > canvas.width) resetBall();

            ai.y += (ball.y - (ai.y + ai.height / 2)) * 0.05;
        }

        function resetBall() {
            ball.x = canvas.width / 2;
            ball.y = canvas.height / 2;
            ball.dx *= -1;
        }

        function gameLoop() {
            draw();
            update();
            requestAnimationFrame(gameLoop);
        }

        document.addEventListener('keydown', e => {
            if (e.key === 'ArrowUp') player.dy = -5;
            if (e.key === 'ArrowDown') player.dy = 5;
        });

        document.addEventListener('keyup', e => {
            if (e.key === 'ArrowUp' || e.key === 'ArrowDown') player.dy = 0;
        });

        gameLoop();
    </script>
</body>
</html>
