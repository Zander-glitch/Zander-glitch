<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Simple Shooting Game</title>
    <style>
        body {
            margin: 0;
            padding: 0;
            overflow: hidden;
            background-color: #f0f0f0;
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
        }

        .game-container {
            position: relative;
            width: 500px;
            height: 500px;
            background-color: #000;
            border: 2px solid #fff;
        }

        .shooter {
            position: absolute;
            bottom: 10px;
            left: 50%;
            width: 50px;
            height: 50px;
            background-color: green;
            transform: translateX(-50%);
        }

        .bullet {
            position: absolute;
            width: 5px;
            height: 10px;
            background-color: red;
            display: none;
        }

        .target {
            position: absolute;
            top: 20px;
            left: 50%;
            width: 50px;
            height: 50px;
            background-color: blue;
            transform: translateX(-50%);
        }
    </style>
</head>
<body>

    <div class="game-container">
        <div class="shooter" id="shooter"></div>
        <div class="target" id="target"></div>
        <div class="bullet" id="bullet"></div>
    </div>

    <script>
        const shooter = document.getElementById("shooter");
        const bullet = document.getElementById("bullet");
        const target = document.getElementById("target");
        const gameContainer = document.querySelector(".game-container");

        let shooterPosition = 225;  // Initial position of shooter (centered)
        let bulletPosition = -10;   // Bullet starts off-screen
        let bulletSpeed = 5;
        let targetPosition = Math.random() * (gameContainer.offsetWidth - 50);
        let score = 0;

        // Set initial positions
        shooter.style.left = `${shooterPosition}px`;
        bullet.style.left = `${shooterPosition + 22}px`; // Position bullet above shooter

        // Move shooter left or right with arrow keys
        document.addEventListener("keydown", (event) => {
            if (event.key === "ArrowLeft" && shooterPosition > 0) {
                shooterPosition -= 10;
            } else if (event.key === "ArrowRight" && shooterPosition < gameContainer.offsetWidth - 50) {
                shooterPosition += 10;
            }

            shooter.style.left = `${shooterPosition}px`;
            bullet.style.left = `${shooterPosition + 22}px`;  // Adjust bullet position with shooter
        });

        // Fire bullet when spacebar is pressed
        document.addEventListener("keydown", (event) => {
            if (event.key === " " && bulletPosition < 0) {
                bulletPosition = 490;  // Reset bullet to bottom of the screen
                bullet.style.display = "block";  // Make bullet visible
                bullet.style.top = `${shooter.offsetTop - 10}px`; // Start bullet just above the shooter
            }
        });

        // Update game state every frame
        function gameLoop() {
            // Move bullet upwards
            if (bulletPosition >= 0) {
                bulletPosition -= bulletSpeed;
                bullet.style.top = `${bulletPosition}px`;

                // Check for collision with target
                if (bulletPosition <= target.offsetTop + 50 && bulletPosition >= target.offsetTop && 
                    bullet.offsetLeft + 5 >= targetPosition && bullet.offsetLeft <= targetPosition + 50) {
                    // Bullet hits the target
                    score++;
                    bulletPosition = -10;  // Reset bullet position off-screen
                    bullet.style.display = "none";  // Hide the bullet
                    targetPosition = Math.random() * (gameContainer.offsetWidth - 50); // Move target to a new position
                    target.style.left = `${targetPosition}px`;
                    alert(`You hit the target! Score: ${score}`);
                }
            }

            // If bullet goes off-screen, hide it
            if (bulletPosition < 0) {
                bullet.style.display = "none";
            }

            requestAnimationFrame(gameLoop);
        }

        gameLoop();  // Start the game loop

    </script>

</body>
</html>
