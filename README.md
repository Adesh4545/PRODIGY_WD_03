# PRODIGY_WD_03
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Tic-Tac-Toe Game</title>
    <style>
        /* Reset and base styles */
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background: linear-gradient(135deg, #1e3c72 0%, #2a5298 100%);
            min-height: 100vh;
            display: flex;
            justify-content: center;
            align-items: center;
            color: white;
        }

        .container {
            background: rgba(255, 255, 255, 0.15);
            backdrop-filter: blur(10px);
            border-radius: 20px;
            padding: 30px;
            box-shadow: 0 8px 32px rgba(0, 0, 0, 0.3);
            border: 1px solid rgba(255, 255, 255, 0.18);
            text-align: center;
            max-width: 450px;
            width: 100%;
        }

        h1 {
            font-size: 2.5rem;
            margin-bottom: 20px;
            color: #fff;
            text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.5);
        }

        .game-mode {
            margin-bottom: 20px;
        }

        .mode-btn {
            background: rgba(255, 255, 255, 0.2);
            border: 2px solid rgba(255, 255, 255, 0.3);
            color: white;
            padding: 12px 24px;
            margin: 0 8px;
            border-radius: 25px;
            cursor: pointer;
            font-size: 16px;
            font-weight: 500;
            transition: all 0.3s ease;
        }

        .mode-btn:hover {
            background: rgba(255, 255, 255, 0.3);
            transform: translateY(-2px);
            box-shadow: 0 4px 12px rgba(0, 0, 0, 0.2);
        }

        .mode-btn.active {
            background: rgba(255, 255, 255, 0.4);
            border-color: rgba(255, 255, 255, 0.7);
        }

        .scoreboard {
            display: flex;
            justify-content: space-around;
            margin: 20px 0;
            padding: 15px;
            background: rgba(255, 255, 255, 0.1);
            border-radius: 15px;
        }

        .score-item {
            text-align: center;
        }

        .score-label {
            font-size: 14px;
            opacity: 0.9;
            margin-bottom: 5px;
        }

        .score-value {
            font-size: 24px;
            font-weight: bold;
            color: #fff;
        }

        .game-info {
            font-size: 18px;
            margin: 20px 0;
            min-height: 25px;
            font-weight: 500;
        }

        .game-board {
            display: grid;
            grid-template-columns: repeat(3, 1fr);
            gap: 8px;
            margin: 25px 0;
            padding: 20px;
            background: rgba(255, 255, 255, 0.1);
            border-radius: 15px;
        }

        .cell {
            width: 90px;
            height: 90px;
            background: rgba(255, 255, 255, 0.2);
            border: 2px solid rgba(255, 255, 255, 0.3);
            border-radius: 12px;
            display: flex;
            justify-content: center;
            align-items: center;
            font-size: 2.5rem;
            font-weight: bold;
            cursor: pointer;
            transition: all 0.3s ease;
            user-select: none;
        }

        .cell:hover:not(.filled) {
            background: rgba(255, 255, 255, 0.3);
            transform: scale(1.05);
            box-shadow: 0 4px 15px rgba(0, 0, 0, 0.2);
        }

        .cell.filled {
            cursor: not-allowed;
        }

        .cell.x {
            color: #ff6b6b;
            text-shadow: 0 0 10px rgba(255, 107, 107, 0.7);
        }

        .cell.o {
            color: #4ecdc4;
            text-shadow: 0 0 10px rgba(78, 205, 196, 0.7);
        }

        .cell.winning {
            animation: winner-glow 1s ease-in-out infinite alternate;
        }

        @keyframes winner-glow {
            from {
                box-shadow: 0 0 20px rgba(255, 255, 255, 0.5);
                transform: scale(1.05);
            }
            to {
                box-shadow: 0 0 30px rgba(255, 255, 255, 0.8);
                transform: scale(1.1);
            }
        }

        .controls {
            margin-top: 20px;
        }

        .btn {
            background: linear-gradient(45deg, #ff6b6b, #ee5a52);
            border: none;
            color: white;
            padding: 12px 30px;
            margin: 0 10px;
            border-radius: 25px;
            cursor: pointer;
            font-size: 16px;
            font-weight: 600;
            transition: all 0.3s ease;
            box-shadow: 0 4px 15px rgba(255, 107, 107, 0.3);
        }

        .btn:hover {
            transform: translateY(-2px);
            box-shadow: 0 6px 20px rgba(255, 107, 107, 0.5);
        }

        .btn:active {
            transform: translateY(0);
        }

        .ai-thinking {
            opacity: 0.7;
            font-style: italic;
        }

        /* Responsive design */
        @media (max-width: 480px) {
            .container {
                padding: 20px;
                margin: 20px;
            }

            .cell {
                width: 70px;
                height: 70px;
                font-size: 2rem;
            }

            h1 {
                font-size: 2rem;
            }

            .mode-btn {
                padding: 10px 20px;
                font-size: 14px;
            }
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>Tic-Tac-Toe</h1>
        
        <div class="game-mode">
            <button class="mode-btn active" data-mode="pvp">Player vs Player</button>
            <button class="mode-btn" data-mode="ai">Player vs AI</button>
        </div>

        <div class="scoreboard">
            <div class="score-item">
                <div class="score-label">Player X</div>
                <div class="score-value" id="scoreX">0</div>
            </div>
            <div class="score-item">
                <div class="score-label">Draws</div>
                <div class="score-value" id="scoreDraw">0</div>
            </div>
            <div class="score-item">
                <div class="score-label">Player O</div>
                <div class="score-value" id="scoreO">0</div>
            </div>
        </div>

        <div class="game-info" id="gameInfo">Player X's turn</div>

        <div class="game-board" id="gameBoard">
            <div class="cell" data-index="0"></div>
            <div class="cell" data-index="1"></div>
            <div class="cell" data-index="2"></div>
            <div class="cell" data-index="3"></div>
            <div class="cell" data-index="4"></div>
            <div class="cell" data-index="5"></div>
            <div class="cell" data-index="6"></div>
            <div class="cell" data-index="7"></div>
            <div class="cell" data-index="8"></div>
        </div>

        <div class="controls">
            <button class="btn" id="resetBtn">New Game</button>
            <button class="btn" id="clearScoreBtn">Clear Score</button>
        </div>
    </div>

    <script>
        // Game Class Definition
        class TicTacToeGame {
            constructor() {
                this.board = ['', '', '', '', '', '', '', '', ''];
                this.currentPlayer = 'X';
                this.gameActive = true;
                this.gameMode = 'pvp'; // 'pvp' or 'ai'
                this.scores = {
                    X: 0,
                    O: 0,
                    draw: 0
                };
                this.winningCombinations = [
                    [0, 1, 2], [3, 4, 5], [6, 7, 8], // Rows
                    [0, 3, 6], [1, 4, 7], [2, 5, 8], // Columns
                    [0, 4, 8], [2, 4, 6]             // Diagonals
                ];
                
                this.initializeGame();
            }

            initializeGame() {
                this.bindEvents();
                this.updateDisplay();
                this.loadScores();
            }

            bindEvents() {
                // Cell click events
                document.querySelectorAll('.cell').forEach(cell => {
                    cell.addEventListener('click', (e) => this.handleCellClick(e));
                });

                // Mode selection
                document.querySelectorAll('.mode-btn').forEach(btn => {
                    btn.addEventListener('click', (e) => this.switchMode(e));
                });

                // Control buttons
                document.getElementById('resetBtn').addEventListener('click', () => this.resetGame());
                document.getElementById('clearScoreBtn').addEventListener('click', () => this.clearScores());
            }

            handleCellClick(event) {
                const cellIndex = parseInt(event.target.getAttribute('data-index'));
                
                if (this.board[cellIndex] !== '' || !this.gameActive) {
                    return;
                }

                this.makeMove(cellIndex);
            }

            makeMove(cellIndex) {
                this.board[cellIndex] = this.currentPlayer;
                this.updateCell(cellIndex);
                
                if (this.checkWin()) {
                    this.handleWin();
                } else if (this.checkDraw()) {
                    this.handleDraw();
                } else {
                    this.switchPlayer();
                    if (this.gameMode === 'ai' && this.currentPlayer === 'O') {
                        this.makeAIMove();
                    }
                }
            }

            updateCell(index) {
                const cell = document.querySelector([data-index="${index}"]);
                cell.textContent = this.currentPlayer;
                cell.classList.add(this.currentPlayer.toLowerCase());
                cell.classList.add('filled');
            }

            checkWin() {
                return this.winningCombinations.some(combination => {
                    const [a, b, c] = combination;
                    if (this.board[a] && this.board[a] === this.board[b] && this.board[a] === this.board[c]) {
                        this.highlightWinningCells(combination);
                        return true;
                    }
                    return false;
                });
            }

            checkDraw() {
                return this.board.every(cell => cell !== '');
            }

            highlightWinningCells(combination) {
                combination.forEach(index => {
                    document.querySelector([data-index="${index}"]).classList.add('winning');
                });
            }

            handleWin() {
                this.gameActive = false;
                this.scores[this.currentPlayer]++;
                document.getElementById('gameInfo').textContent = Player ${this.currentPlayer} wins!;
                this.updateScoreDisplay();
                this.saveScores();
            }

            handleDraw() {
                this.gameActive = false;
                this.scores.draw++;
                document.getElementById('gameInfo').textContent = "It's a draw!";
                this.updateScoreDisplay();
                this.saveScores();
            }

            switchPlayer() {
                this.currentPlayer = this.currentPlayer === 'X' ? 'O' : 'X';
                this.updateGameInfo();
            }

            updateGameInfo() {
                if (this.gameActive) {
                    const playerName = this.gameMode === 'ai' && this.currentPlayer === 'O' ? 'AI' : Player ${this.currentPlayer};
                    document.getElementById('gameInfo').textContent = ${playerName}'s turn;
                }
            }

            // AI Logic
            makeAIMove() {
                document.getElementById('gameInfo').textContent = 'AI is thinking...';
                document.getElementById('gameInfo').classList.add('ai-thinking');
                
                setTimeout(() => {
                    const move = this.getBestMove();
                    if (move !== -1) {
                        this.makeMove(move);
                    }
                    document.getElementById('gameInfo').classList.remove('ai-thinking');
                }, 800);
            }

            getBestMove() {
                // Try to win
                for (let i = 0; i < 9; i++) {
                    if (this.board[i] === '') {
                        this.board[i] = 'O';
                        if (this.checkWin()) {
                            this.board[i] = '';
                            return i;
                        }
                        this.board[i] = '';
                    }
                }

                // Try to block player from winning
                for (let i = 0; i < 9; i++) {
                    if (this.board[i] === '') {
                        this.board[i] = 'X';
                        if (this.checkWin()) {
                            this.board[i] = '';
                            return i;
                        }
                        this.board[i] = '';
                    }
                }

                // Take center if available
                if (this.board[4] === '') {
                    return 4;
                }

                // Take corners
                const corners = [0, 2, 6, 8];
                const availableCorners = corners.filter(i => this.board[i] === '');
                if (availableCorners.length > 0) {
                    return availableCorners[Math.floor(Math.random() * availableCorners.length)];
                }

                // Take any available spot
                const availableSpots = this.board.map((cell, index) => cell === '' ? index : null).filter(i => i !== null);
                return availableSpots.length > 0 ? availableSpots[Math.floor(Math.random() * availableSpots.length)] : -1;
            }

            switchMode(event) {
                document.querySelectorAll('.mode-btn').forEach(btn => btn.classList.remove('active'));
                event.target.classList.add('active');
                
                this.gameMode = event.target.getAttribute('data-mode');
                this.updateModeLabels();
                this.resetGame();
            }

            updateModeLabels() {
                const labels = document.querySelectorAll('.score-label');
                if (this.gameMode === 'ai') {
                    labels[0].textContent = 'Player';
                    labels[2].textContent = 'AI';
                } else {
                    labels[0].textContent = 'Player X';
                    labels[2].textContent = 'Player O';
                }
            }

            resetGame() {
                this.board = ['', '', '', '', '', '', '', '', ''];
                this.currentPlayer = 'X';
                this.gameActive = true;
                
                document.querySelectorAll('.cell').forEach(cell => {
                    cell.textContent = '';
                    cell.classList.remove('x', 'o', 'filled', 'winning');
                });
                
                this.updateDisplay();
            }

            updateDisplay() {
                this.updateGameInfo();
                this.updateScoreDisplay();
            }

            updateScoreDisplay() {
                document.getElementById('scoreX').textContent = this.scores.X;
                document.getElementById('scoreO').textContent = this.scores.O;
                document.getElementById('scoreDraw').textContent = this.scores.draw;
            }

            clearScores() {
                this.scores = { X: 0, O: 0, draw: 0 };
                this.updateScoreDisplay();
                this.saveScores();
            }

            saveScores() {
                try {
                    const scoresData = JSON.stringify(this.scores);
                    // For environments that support localStorage
                    if (typeof localStorage !== 'undefined') {
                        localStorage.setItem('ticTacToeScores', scoresData);
                    }
                } catch (e) {
                    // Fallback for environments without localStorage
                    console.log('Score saving not available in this environment');
                }
            }

            loadScores() {
                try {
                    if (typeof localStorage !== 'undefined') {
                        const saved = localStorage.getItem('ticTacToeScores');
                        if (saved) {
                            this.scores = JSON.parse(saved);
                            this.updateScoreDisplay();
                        }
                    }
                } catch (e) {
                    console.log('Score loading not available in this environment');
                }
            }
        }

        // Initialize the game when DOM is loaded
        document.addEventListener('DOMContentLoaded', function() {
            window.game = new TicTacToeGame();
        });

        // For environments that might load scripts before DOM
        if (document.readyState === 'loading') {
            document.addEventListener('DOMContentLoaded', function() {
                if (!window.game) {
                    window.game = new TicTacToeGame();
                }
            });
        } else {
            window.game = new TicTacToeGame();
        }
    </script>
</body>
</html>
