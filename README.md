<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Tic Tac Toe - Game App</title>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Poppins:wght@600&display=swap');
        
        * { margin: 0; padding: 0; box-sizing: border-box; }
        
        body {
            font-family: 'Poppins', sans-serif;
            background: linear-gradient(135deg, #0f0c29, #302b63, #24243e);
            color: white;
            height: 100vh;
            display: flex;
            align-items: center;
            justify-content: center;
            overflow: hidden;
        }
        
        .game-container {
            text-align: center;
            background: rgba(255, 255, 255, 0.1);
            padding: 30px 20px;
            border-radius: 20px;
            backdrop-filter: blur(10px);
            box-shadow: 0 20px 40px rgba(0, 0, 0, 0.5);
            max-width: 420px;
            width: 95%;
        }
        
        h1 {
            font-size: 2.8rem;
            margin-bottom: 10px;
            text-shadow: 0 0 20px #00ffcc;
        }
        
        .subtitle { font-size: 1.1rem; margin-bottom: 25px; opacity: 0.9; }
        
        .board {
            display: grid;
            grid-template-columns: repeat(3, 1fr);
            gap: 12px;
            max-width: 360px;
            margin: 0 auto 30px;
            padding: 15px;
            background: rgba(0, 0, 0, 0.3);
            border-radius: 15px;
        }
        
        .cell {
            width: 100px;
            height: 100px;
            background: #1e1b2f;
            border-radius: 12px;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 3.5rem;
            font-weight: bold;
            cursor: pointer;
            transition: all 0.3s ease;
            box-shadow: 0 5px 15px rgba(0, 0, 0, 0.4);
        }
        
        .cell:hover { background: #2a2750; transform: scale(1.05); }
        .cell.x { color: #00ffcc; }
        .cell.o { color: #ff00aa; }
        
        .status { font-size: 1.4rem; margin-bottom: 20px; min-height: 50px; }
        
        .player-info {
            display: flex;
            justify-content: center;
            gap: 30px;
            margin-bottom: 25px;
        }
        
        .player { display: flex; align-items: center; gap: 10px; font-size: 1.3rem; }
        .player.x::before { content: "❌"; font-size: 1.8rem; }
        .player.o::before { content: "⭕"; font-size: 1.8rem; }
        
        button {
            background: linear-gradient(45deg, #00ffcc, #ff00aa);
            color: #000;
            border: none;
            padding: 15px 40px;
            font-size: 1.3rem;
            font-weight: 600;
            border-radius: 50px;
            cursor: pointer;
            box-shadow: 0 10px 25px rgba(0, 255, 204, 0.4);
            transition: all 0.3s;
        }
        
        button:hover {
            transform: translateY(-5px);
            box-shadow: 0 15px 30px rgba(255, 0, 170, 0.4);
        }
        
        .score {
            display: flex;
            justify-content: center;
            gap: 40px;
            margin-top: 20px;
            font-size: 1.2rem;
        }
        
        @media (max-width: 480px) {
            .cell { width: 85px; height: 85px; font-size: 3rem; }
            h1 { font-size: 2.3rem; }
        }
    </style>
</head>
<body>
    <div class="game-container">
        <h1>🎮 TIC TAC TOE</h1>
        <p class="subtitle">Simple Game App</p>
        
        <div class="player-info">
            <div class="player x" id="playerX">Player X (You)</div>
            <div class="player o" id="playerO">Player O (Friend)</div>
        </div>
        
        <div id="status" class="status">Player X's Turn</div>
        
        <div id="board" class="board"></div>
        
        <button onclick="resetGame()">Start New Game</button>
        <div class="score">
            <div>Player X: <span id="scoreX">0</span></div>
            <div>Player O: <span id="scoreO">0</span></div>
            <div>Draw: <span id="scoreDraw">0</span></div>
        </div>
    </div>

    <script>
        let board = ['', '', '', '', '', '', '', '', ''];
        let currentPlayer = 'X';
        let gameActive = true;
        let scores = { X: 0, O: 0, draw: 0 };
        
        const winningConditions = [
            [0,1,2],[3,4,5],[6,7,8],
            [0,3,6],[1,4,7],[2,5,8],
            [0,4,8],[2,4,6]
        ];
        
        const boardElement = document.getElementById('board');
        const statusElement = document.getElementById('status');
        
        function createBoard() {
            boardElement.innerHTML = '';
            for (let i = 0; i < 9; i++) {
                const cell = document.createElement('div');
                cell.classList.add('cell');
                cell.setAttribute('data-index', i);
                cell.addEventListener('click', handleCellClick);
                boardElement.appendChild(cell);
            }
        }
        
        function handleCellClick(e) {
            const index = parseInt(e.target.getAttribute('data-index'));
            
            if (board[index] !== '' || !gameActive) return;
            
            board[index] = currentPlayer;
            e.target.textContent = currentPlayer;
            e.target.classList.add(currentPlayer.toLowerCase());
            
            if (checkWin()) {
                gameActive = false;
                statusElement.innerHTML = `<span style="color:#00ffcc">🎉 Player ${currentPlayer} Wins!</span>`;
                scores[currentPlayer]++;
                updateScores();
                return;
            }
            
            if (board.every(cell => cell !== '')) {
                gameActive = false;
                statusElement.innerHTML = `<span style="color:#ffcc00">😕 It's a Draw!</span>`;
                scores.draw++;
                updateScores();
                return;
            }
            
            currentPlayer = currentPlayer === 'X' ? 'O' : 'X';
            statusElement.textContent = `Player ${currentPlayer}'s Turn`;
        }
        
        function checkWin() {
            return winningConditions.some(condition => {
                return condition.every(index => board[index] === currentPlayer);
            });
        }
        
        function updateScores() {
            document.getElementById('scoreX').textContent = scores.X;
            document.getElementById('scoreO').textContent = scores.O;
            document.getElementById('scoreDraw').textContent = scores.draw;
        }
        
        function resetGame() {
            board = ['', '', '', '', '', '', '', '', ''];
            currentPlayer = 'X';
            gameActive = true;
            statusElement.textContent = "Player X's Turn";
            createBoard();
        }
        
        function initGame() {
            createBoard();
            updateScores();
        }
        
        window.onload = initGame;
    </script>
</body>
</html>
