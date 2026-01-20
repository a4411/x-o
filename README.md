# x-o
xxxoooo
<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>إكس أو المحترفين | Pro Tic-Tac-Toe</title>
    <link rel="stylesheet" href="style.css">
</head>
<body>

    <div class="game-container">
        <div id="main-menu" class="menu-screen active">
            <h1>إكس أو <span class="pro">PRO</span></h1>
            <button onclick="startMode('ai')">اللعب ضد الذكاء الاصطناعي</button>
            <button onclick="startMode('local')">لاعب ضد لاعب (Local)</button>
            <button onclick="showOnlineMenu()">أونلاين (قريباً)</button>
            <div class="stats">أعلى نتيجة: <span id="high-score">0</span></div>
        </div>

        <div id="game-board-screen" class="menu-screen">
            <div class="status-bar">
                <div class="player active" id="p1-status">اللاعب 1 (X)</div>
                <div class="vs">VS</div>
                <div class="player" id="p2-status">الخصم (O)</div>
            </div>
            
            <div class="board" id="board">
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

            <button class="back-btn" onclick="goToMenu()">العودة للقائمة</button>
        </div>
    </div>

    <div id="result-overlay" class="overlay">
        <div class="result-box">
            <h2 id="result-text">فاز اللاعب X!</h2>
            <button onclick="restartGame()">لعب مجدداً</button>
        </div>
    </div>

    <script src="script.js"></script>
</body>
</html>
let board = ["", "", "", "", "", "", "", "", ""];
let currentPlayer = "X";
let gameMode = 'ai'; // 'ai' or 'local'
let isGameActive = true;

const cells = document.querySelectorAll('.cell');
const statusText = document.getElementById('result-text');

// بدء اللعبة
function startMode(mode) {
    gameMode = mode;
    document.getElementById('main-menu').classList.remove('active');
    document.getElementById('game-board-screen').classList.add('active');
    restartGame();
}

cells.forEach(cell => cell.addEventListener('click', handleCellClick));

function handleCellClick(e) {
    const index = e.target.dataset.index;
    if (board[index] !== "" || !isGameActive) return;

    makeMove(index, currentPlayer);

    if (checkWin(board, currentPlayer)) {
        endGame(`فاز اللاعب ${currentPlayer}!`);
    } else if (board.every(cell => cell !== "")) {
        endGame("تعادل!");
    } else {
        currentPlayer = currentPlayer === "X" ? "O" : "X";
        if (gameMode === 'ai' && currentPlayer === "O") {
            setTimeout(aiMove, 500);
        }
    }
}

function makeMove(index, player) {
    board[index] = player;
    cells[index].innerText = player;
    cells[index].classList.add(player.toLowerCase());
}

// ذكاء اصطناعي (Minimax)
function aiMove() {
    let bestScore = -Infinity;
    let move;
    for (let i = 0; i < 9; i++) {
        if (board[i] === "") {
            board[i] = "O";
            let score = minimax(board, 0, false);
            board[i] = "";
            if (score > bestScore) {
                bestScore = score;
                move = i;
            }
        }
    }
    makeMove(move, "O");
    if (checkWin(board, "O")) {
        endGame("الذكاء الاصطناعي فاز!");
    } else {
        currentPlayer = "X";
    }
}

function minimax(board, depth, isMaximizing) {
    if (checkWin(board, "O")) return 10;
    if (checkWin(board, "X")) return -10;
    if (board.every(s => s !== "")) return 0;

    if (isMaximizing) {
        let bestScore = -Infinity;
        for (let i = 0; i < 9; i++) {
            if (board[i] === "") {
                board[i] = "O";
                let score = minimax(board, depth + 1, false);
                board[i] = "";
                bestScore = Math.max(score, bestScore);
            }
        }
        return bestScore;
    } else {
        let bestScore = Infinity;
        for (let i = 0; i < 9; i++) {
            if (board[i] === "") {
                board[i] = "X";
                let score = minimax(board, depth + 1, true);
                board[i] = "";
                bestScore = Math.min(score, bestScore);
            }
        }
        return bestScore;
    }
}

function checkWin(b, p) {
    const winPatterns = [
        [0,1,2], [3,4,5], [6,7,8], [0,3,6], [1,4,7], [2,5,8], [0,4,8], [2,4,6]
    ];
    return winPatterns.some(pattern => pattern.every(i => b[i] === p));
}

function endGame(msg) {
    isGameActive = false;
    document.getElementById('result-overlay').style.display = 'flex';
    statusText.innerText = msg;
}

function restartGame() {
    board = ["", "", "", "", "", "", "", "", ""];
    isGameActive = true;
    currentPlayer = "X";
    cells.forEach(cell => {
        cell.innerText = "";
        cell.classList.remove('x', 'o');
    });
    document.getElementById('result-overlay').style.display = 'none';
}

function goToMenu() {
    document.getElementById('main-menu').classList.add('active');
    document.getElementById('game-board-screen').classList.r:root {
    --bg: #0f172a;
    --card: #1e293b;
    --primary: #38bdf8;
    --secondary: #f472b6;
    --text: #f8fafc;
}

body {
    margin: 0;
    font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
    background-color: var(--bg);
    color: var(--text);
    display: flex;
    justify-content: center;
    align-items: center;
    height: 100vh;
    overflow: hidden;
}

.game-container {
    background: var(--card);
    padding: 2rem;
    border-radius: 20px;
    box-shadow: 0 10px 30px rgba(0,0,0,0.5);
    text-align: center;
    width: 90%;
    max-width: 400px;
}

.menu-screen { display: none; }
.menu-screen.active { display: block; }

h1 .pro { color: var(--primary); font-style: italic; }

button {
    width: 100%;
    padding: 12px;
    margin: 10px 0;
    border: none;
    border-radius: 10px;
    background: var(--primary);
    color: white;
    font-size: 1.1rem;
    cursor: pointer;
    transition: 0.3s;
}

button:hover { transform: translateY(-3px); filter: brightness(1.1); }

/* ساحة اللعب */
.board {
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    gap: 10px;
    margin: 20px 0;
}

.cell {
    aspect-ratio: 1;
    background: rgba(255,255,255,0.05);
    border-radius: 10px;
    display: flex;
    justify-content: center;
    align-items: center;
    font-size: 2.5rem;
    font-weight: bold;
    cursor: pointer;
}

.cell:hover { background: rgba(255,255,255,0.1); }
.cell.x { color: var(--primary); }
.cell.o { color: var(--secondary); }

/* الأوضاع */
.overlay {
    position: fixed; top: 0; left: 0; width: 100%; height: 100%;
    background: rgba(0,0,0,0.85);
    display: none; justify-content: center; align-items: center;
}
 emove('active');
}
