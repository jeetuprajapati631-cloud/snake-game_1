# snake-game_1

<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Snake Game</title>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <script src="https://cdnjs.cloudflare.com/ajax/libs/howler/2.2.3/howler.min.js"></script>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            user-select: none;
        }

        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background: linear-gradient(135deg, #1a2a6c, #b21f1f, #fdbb2d);
            min-height: 100vh;
            display: flex;
            justify-content: center;
            align-items: center;
            color: white;
            overflow: hidden;
            padding: 20px;
        }

        .game-container {
            position: relative;
            max-width: 500px;
            width: 100%;
            display: flex;
            flex-direction: column;
            align-items: center;
            gap: 20px;
            padding: 20px;
            background: rgba(0, 0, 0, 0.7);
            border-radius: 20px;
            box-shadow: 0 10px 30px rgba(0, 0, 0, 0.5);
            backdrop-filter: blur(10px);
            border: 2px solid rgba(255, 255, 255, 0.1);
        }

        h1 {
            font-size: 2.5rem;
            text-align: center;
            text-shadow: 0 2px 10px rgba(0, 0, 0, 0.3);
            background: linear-gradient(to right, #ff7e5f, #feb47b);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            margin-bottom: 10px;
        }

        .game-info {
            display: flex;
            justify-content: space-between;
            width: 100%;
            padding: 0 10px;
            font-size: 1.2rem;
            font-weight: bold;
        }

        .game-board {
            position: relative;
            width: 100%;
            aspect-ratio: 1/1;
            background: rgba(10, 20, 30, 0.8);
            border-radius: 10px;
            overflow: hidden;
            border: 2px solid rgba(255, 255, 255, 0.1);
            box-shadow: inset 0 0 20px rgba(0, 0, 0, 0.5);
        }

        canvas {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
        }

        .screen {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            gap: 20px;
            background: rgba(0, 0, 0, 0.8);
            z-index: 10;
            text-align: center;
            padding: 20px;
            transition: opacity 0.3s;
        }

        .screen.hidden {
            opacity: 0;
            pointer-events: none;
        }

        .screen h2 {
            font-size: 2rem;
            color: #ff7e5f;
        }

        .screen p {
            font-size: 1.2rem;
            max-width: 80%;
        }

        .btn {
            padding: 12px 30px;
            font-size: 1.2rem;
            background: linear-gradient(to right, #ff7e5f, #feb47b);
            border: none;
            border-radius: 50px;
            color: white;
            cursor: pointer;
            font-weight: bold;
            transition: transform 0.2s, box-shadow 0.2s;
            box-shadow: 0 4px 10px rgba(0, 0, 0, 0.3);
        }

        .btn:hover {
            transform: translateY(-3px);
            box-shadow: 0 6px 15px rgba(0, 0, 0, 0.4);
        }

        .btn:active {
            transform: translateY(1px);
            box-shadow: 0 2px 5px rgba(0, 0, 0, 0.3);
        }

        .controls {
            display: flex;
            flex-direction: column;
            align-items: center;
            gap: 10px;
            width: 100%;
        }

        .control-row {
            display: flex;
            justify-content: center;
            gap: 20px;
        }

        .control-btn {
            width: 60px;
            height: 60px;
            display: flex;
            justify-content: center;
            align-items: center;
            background: rgba(255, 255, 255, 0.1);
            border-radius: 50%;
            font-size: 1.5rem;
            cursor: pointer;
            transition: background 0.2s;
            box-shadow: 0 4px 10px rgba(0, 0, 0, 0.3);
            border: 2px solid rgba(255, 255, 255, 0.2);
        }

        .control-btn:active {
            background: rgba(255, 255, 255, 0.3);
        }

        .snake-segment {
            position: absolute;
            background: #4CAF50;
            border-radius: 5px;
            border: 1px solid #388E3C;
        }

        .snake-head {
            background: #388E3C;
            border-radius: 50% 50% 5px 5px;
        }

        .food {
            position: absolute;
            width: 15px;
            height: 15px;
            background: #FF5252;
            border-radius: 50%;
            box-shadow: 0 0 10px #FF5252, 0 0 20px #FF5252;
            animation: pulse 1s infinite alternate;
        }

        @keyframes pulse {
            from { transform: scale(1); }
            to { transform: scale(1.2); }
        }

        .instructions {
            margin-top: 10px;
            font-size: 0.9rem;
            color: rgba(255, 255, 255, 0.7);
            text-align: center;
        }

        .high-score {
            color: #feb47b;
            font-weight: bold;
        }

        .mobile-only {
            display: none;
        }

        @media (max-width: 600px) {
            h1 {
                font-size: 2rem;
            }
            
            .screen h2 {
                font-size: 1.8rem;
            }
            
            .mobile-only {
                display: block;
            }
            
            .desktop-only {
                display: none;
            }
            
            .controls {
                margin-top: 10px;
            }
        }

        @media (max-height: 700px) {
            .game-container {
                transform: scale(0.9);
            }
        }
    </style>
</head>
<body>
    <div class="game-container">
        <h1>Snake Game</h1>
        
        <div class="game-info">
            <div class="score">Score: <span id="score">0</span></div>
            <div class="high-score">High Score: <span id="high-score">0</span></div>
        </div>
        
        <div class="game-board">
            <canvas id="game-canvas"></canvas>
            
            <div id="start-screen" class="screen">
                <h2>Snake Game</h2>
                <p>Eat as much food as you can without hitting the walls or yourself!</p>
                <button id="start-btn" class="btn">Start Game</button>
                <p class="desktop-only">Controls: Use Arrow Keys or WASD</p>
                <p class="mobile-only">Use the buttons below to control the snake</p>
            </div>
            
            <div id="game-over-screen" class="screen hidden">
                <h2>Game Over!</h2>
                <p>Your score: <span id="final-score">0</span></p>
                <p>High score: <span id="final-high-score">0</span></p>
                <button id="restart-btn" class="btn">Play Again</button>
            </div>
        </div>
        
        <div class="controls">
            <div class="control-row">
                <div class="control-btn" id="up-btn"><i class="fas fa-arrow-up"></i></div>
            </div>
            <div class="control-row">
                <div class="control-btn" id="left-btn"><i class="fas fa-arrow-left"></i></div>
                <div class="control-btn" id="down-btn"><i class="fas fa-arrow-down"></i></div>
                <div class="control-btn" id="right-btn"><i class="fas fa-arrow-right"></i></div>
            </div>
        </div>
        
        <div class="instructions">
            <p>Collect the red food to grow your snake and earn points!</p>
        </div>
    </div>

    <script>
        // Game constants and variables
        const GRID_SIZE = 20;
        const CELL_SIZE = 20;
        const INITIAL_SPEED = 150;
        const SPEED_INCREMENT = 2;
        
        let canvas, ctx;
        let snake = [];
        let food = {x: 0, y: 0};
        let direction = 'right';
        let nextDirection = 'right';
        let score = 0;
        let highScore = localStorage.getItem('snakeHighScore') || 0;
        let gameRunning = false;
        let gameLoop;
        let speed = INITIAL_SPEED;
        
        // Sound effects
        const sounds = {
            eat: new Howl({ src: ['https://assets.mixkit.co/sfx/preview/mixkit-game-ball-tap-2073.mp3'] }),
            gameOver: new Howl({ src: ['https://assets.mixkit.co/sfx/preview/mixkit-losing-drums-2023.mp3'] }),
            move: new Howl({ src: ['https://assets.mixkit.co/sfx/preview/mixkit-arcade-game-jump-coin-216.mp3'], volume: 0.3 })
        };
        
        // Initialize game
        function init() {
            canvas = document.getElementById('game-canvas');
            ctx = canvas.getContext('2d');
            
            // Set canvas size
            const board = document.querySelector('.game-board');
            canvas.width = board.clientWidth;
            canvas.height = board.clientHeight;
            
            // Set high score display
            document.getElementById('high-score').textContent = highScore;
            document.getElementById('final-high-score').textContent = highScore;
            
            // Event listeners
            document.getElementById('start-btn').addEventListener('click', startGame);
            document.getElementById('restart-btn').addEventListener('click', startGame);
            
            // Direction buttons
            document.getElementById('up-btn').addEventListener('click', () => changeDirection('up'));
            document.getElementById('down-btn').addEventListener('click', () => changeDirection('down'));
            document.getElementById('left-btn').addEventListener('click', () => changeDirection('left'));
            document.getElementById('right-btn').addEventListener('click', () => changeDirection('right'));
            
            // Keyboard controls
            document.addEventListener('keydown', handleKeydown);
            
            // Initialize the game board
            drawBoard();
        }
        
        // Start the game
        function startGame() {
            // Hide screens
            document.getElementById('start-screen').classList.add('hidden');
            document.getElementById('game-over-screen').classList.add('hidden');
            
            // Reset game state
            snake = [
                {x: 5, y: 10},
                {x: 4, y: 10},
                {x: 3, y: 10}
            ];
            
            direction = 'right';
            nextDirection = 'right';
            score = 0;
            speed = INITIAL_SPEED;
            updateScore();
            
            // Place first food
            placeFood();
            
            // Start game loop
            gameRunning = true;
            clearInterval(gameLoop);
            gameLoop = setInterval(gameUpdate, speed);
        }
        
        // Game update loop
        function gameUpdate() {
            // Update direction
            direction = nextDirection;
            
            // Move snake
            moveSnake();
            
            // Check collisions
            if (checkCollision()) {
                gameOver();
                return;
            }
            
            // Check if snake ate food
            if (snake[0].x === food.x && snake[0].y === food.y) {
                // Grow snake
                snake.push({...snake[snake.length - 1]});
                
                // Increase score
                score += 10;
                updateScore();
                
                // Increase speed
                if (speed > 50) {
                    speed -= SPEED_INCREMENT;
                    clearInterval(gameLoop);
                    gameLoop = setInterval(gameUpdate, speed);
                }
                
                // Play sound
                sounds.eat.play();
                
                // Place new food
                placeFood();
            } else {
                // Play move sound
                sounds.move.play();
            }
            
            // Draw everything
            draw();
        }
        
        // Move the snake
        function moveSnake() {
            // Create new head based on direction
            const head = {...snake[0]};
            
            switch(direction) {
                case 'up': head.y--; break;
                case 'down': head.y++; break;
                case 'left': head.x--; break;
                case 'right': head.x++; break;
            }
            
            // Add new head to beginning
            snake.unshift(head);
            
            // Remove tail
            snake.pop();
        }
        
        // Check for collisions
        function checkCollision() {
            const head = snake[0];
            
            // Wall collision
            if (head.x < 0 || head.x >= GRID_SIZE || head.y < 0 || head.y >= GRID_SIZE) {
                return true;
            }
            
            // Self collision (skip first 3 segments to prevent false collision)
            for (let i = 3; i < snake.length; i++) {
                if (head.x === snake[i].x && head.y === snake[i].y) {
                    return true;
                }
            }
            
            return false;
        }
        
        // Draw everything on canvas
        function draw() {
            // Clear canvas
            ctx.clearRect(0, 0, canvas.width, canvas.height);
            
            // Draw snake
            snake.forEach((segment, index) => {
                drawSegment(segment, index === 0);
            });
            
            // Draw food
            drawFood();
        }
        
        // Draw a snake segment
        function drawSegment(segment, isHead) {
            const x = segment.x * CELL_SIZE;
            const y = segment.y * CELL_SIZE;
            
            if (isHead) {
                ctx.fillStyle = '#388E3C';
                ctx.beginPath();
                ctx.arc(x + CELL_SIZE/2, y + CELL_SIZE/2, CELL_SIZE/2, 0, Math.PI * 2);
                ctx.fill();
                
                // Draw eyes
                ctx.fillStyle = 'black';
                if (direction === 'right') {
                    ctx.beginPath();
                    ctx.arc(x + CELL_SIZE * 0.7, y + CELL_SIZE * 0.3, 3, 0, Math.PI * 2);
                    ctx.arc(x + CELL_SIZE * 0.7, y + CELL_SIZE * 0.7, 3, 0, Math.PI * 2);
                    ctx.fill();
                } else if (direction === 'left') {
                    ctx.beginPath();
                    ctx.arc(x + CELL_SIZE * 0.3, y + CELL_SIZE * 0.3, 3, 0, Math.PI * 2);
                    ctx.arc(x + CELL_SIZE * 0.3, y + CELL_SIZE * 0.7, 3, 0, Math.PI * 2);
                    ctx.fill();
                } else if (direction === 'up') {
                    ctx.beginPath();
                    ctx.arc(x + CELL_SIZE * 0.3, y + CELL_SIZE * 0.3, 3, 0, Math.PI * 2);
                    ctx.arc(x + CELL_SIZE * 0.7, y + CELL_SIZE * 0.3, 3, 0, Math.PI * 2);
                    ctx.fill();
                } else if (direction === 'down') {
                    ctx.beginPath();
                    ctx.arc(x + CELL_SIZE * 0.3, y + CELL_SIZE * 0.7, 3, 0, Math.PI * 2);
                    ctx.arc(x + CELL_SIZE * 0.7, y + CELL_SIZE * 0.7, 3, 0, Math.PI * 2);
                    ctx.fill();
                }
            } else {
                ctx.fillStyle = '#4CAF50';
                ctx.fillRect(x, y, CELL_SIZE, CELL_SIZE);
                
                ctx.fillStyle = '#388E3C';
                ctx.fillRect(x + 2, y + 2, CELL_SIZE - 4, CELL_SIZE - 4);
            }
        }
        
        // Draw food
        function drawFood() {
            const x = food.x * CELL_SIZE;
            const y = food.y * CELL_SIZE;
            
            ctx.fillStyle = '#FF5252';
            ctx.beginPath();
            ctx.arc(x + CELL_SIZE/2, y + CELL_SIZE/2, CELL_SIZE/2 - 2, 0, Math.PI * 2);
            ctx.fill();
            
            // Draw shine effect
            ctx.fillStyle = '#FF8A80';
            ctx.beginPath();
            ctx.arc(x + CELL_SIZE * 0.3, y + CELL_SIZE * 0.3, 3, 0, Math.PI * 2);
            ctx.fill();
        }
        
        // Draw initial board
        function drawBoard() {
            ctx.fillStyle = '#0a141e';
            ctx.fillRect(0, 0, canvas.width, canvas.height);
            
            // Draw grid
            ctx.strokeStyle = 'rgba(255, 255, 255, 0.05)';
            ctx.lineWidth = 1;
            
            for (let x = 0; x <= GRID_SIZE; x++) {
                ctx.beginPath();
                ctx.moveTo(x * CELL_SIZE, 0);
                ctx.lineTo(x * CELL_SIZE, GRID_SIZE * CELL_SIZE);
                ctx.stroke();
            }
            
            for (let y = 0; y <= GRID_SIZE; y++) {
                ctx.beginPath();
                ctx.moveTo(0, y * CELL_SIZE);
                ctx.lineTo(GRID_SIZE * CELL_SIZE, y * CELL_SIZE);
                ctx.stroke();
            }
        }
        
        // Place food at random position
        function placeFood() {
            // Find empty position
            let newFood;
            let positionValid = false;
            
            while (!positionValid) {
                newFood = {
                    x: Math.floor(Math.random() * GRID_SIZE),
                    y: Math.floor(Math.random() * GRID_SIZE)
                };
                
                // Check if position is on snake
                positionValid = true;
                for (const segment of snake) {
                    if (segment.x === newFood.x && segment.y === newFood.y) {
                        positionValid = false;
                        break;
                    }
                }
            }
            
            food = newFood;
        }
        
        // Update score display
        function updateScore() {
            document.getElementById('score').textContent = score;
        }
        
        // Handle game over
        function gameOver() {
            gameRunning = false;
            clearInterval(gameLoop);
            
            // Update high score
            if (score > highScore) {
                highScore = score;
                localStorage.setItem('snakeHighScore', highScore);
            }
            
            // Update game over screen
            document.getElementById('final-score').textContent = score;
            document.getElementById('final-high-score').textContent = highScore;
            
            // Show game over screen
            document.getElementById('game-over-screen').classList.remove('hidden');
            
            // Play sound
            sounds.gameOver.play();
        }
        
        // Handle keyboard input
        function handleKeydown(e) {
            if (!gameRunning) return;
            
            switch(e.key) {
                case 'ArrowUp':
                case 'w':
                case 'W':
                    if (direction !== 'down') nextDirection = 'up';
                    break;
                case 'ArrowDown':
                case 's':
                case 'S':
                    if (direction !== 'up') nextDirection = 'down';
                    break;
                case 'ArrowLeft':
                case 'a':
                case 'A':
                    if (direction !== 'right') nextDirection = 'left';
                    break;
                case 'ArrowRight':
                case 'd':
                case 'D':
                    if (direction !== 'left') nextDirection = 'right';
                    break;
            }
        }
        
        // Change direction with buttons
        function changeDirection(newDirection) {
            if (!gameRunning) return;
            
            if (
                (newDirection === 'up' && direction !== 'down') ||
                (newDirection === 'down' && direction !== 'up') ||
                (newDirection === 'left' && direction !== 'right') ||
                (newDirection === 'right' && direction !== 'left')
            ) {
                nextDirection = newDirection;
            }
        }
        
        // Initialize the game when page loads
        window.addEventListener('load', init);
        
        // Handle window resize
        window.addEventListener('resize', () => {
            if (canvas) {
                const board = document.querySelector('.game-board');
                canvas.width = board.clientWidth;
                canvas.height = board.clientHeight;
                drawBoard();
                if (gameRunning) draw();
            }
        });
    </script>
</body>
</html>
