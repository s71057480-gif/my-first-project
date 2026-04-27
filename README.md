<!DOCTYPE html>
<html>
<head>
    <title>Connect Four GitHub Game</title>
    <style>
        body { display: flex; flex-direction: column; align-items: center; background: #2c3e50; color: white; font-family: sans-serif; }
        #board { display: grid; grid-template-columns: repeat(7, 50px); grid-gap: 5px; background: #34495e; padding: 10px; border-radius: 10px; }
        .cell { width: 50px; height: 50px; background: white; border-radius: 50%; cursor: pointer; }
        .cell.player1 { background: #e74c3c; } /* 빨간색 칩 */
        .cell.player2 { background: #f1c40f; } /* 노란색 칩 */
    </style>
</head>
<body>
    <h1>Connect Four</h1>
    <div id="board"></div>
    <p id="status">플레이어 1의 차례입니다 (빨간색)</p>

    <script>
        const ROWS = 6, COLS = 7;
        let currentPlayer = 1;
        let board = Array.from({ length: ROWS }, () => Array(COLS).fill(0));

        const boardEl = document.getElementById('board');
        const statusEl = document.getElementById('status');

        function createBoard() {
            boardEl.innerHTML = '';
            for (let r = 0; r < ROWS; r++) {
                for (let c = 0; c < COLS; c++) {
                    const cell = document.createElement('div');
                    cell.classList.add('cell');
                    if (board[r][c] === 1) cell.classList.add('player1');
                    if (board[r][c] === 2) cell.classList.add('player2');
                    cell.onclick = () => dropPiece(c);
                    boardEl.appendChild(cell);
                }
            }
        }

        function dropPiece(c) {
            for (let r = ROWS - 1; r >= 0; r--) {
                if (board[r][c] === 0) {
                    board[r][c] = currentPlayer;
                    if (checkWin(r, c)) {
                        createBoard();
                        alert(`플레이어 ${currentPlayer} 승리!`);
                        board = Array.from({ length: ROWS }, () => Array(COLS).fill(0));
                    }
                    currentPlayer = currentPlayer === 1 ? 2 : 1;
                    statusEl.innerText = `플레이어 ${currentPlayer}의 차례입니다`;
                    createBoard();
                    return;
                }
            }
        }

        function checkWin(r, c) {
            const directions = [[0,1], [1,0], [1,1], [1,-1]];
            return directions.some(([dr, dc]) => {
                let count = 1;
                [[1,1], [-1,-1]].forEach(([s]) => {
                    let nr = r + dr*s, nc = c + dc*s;
                    while(nr>=0 && nr<ROWS && nc>=0 && nc<COLS && board[nr][nc] === currentPlayer) {
                        count++; nr += dr*s; nc += dc*s;
                    }
                });
                return count >= 4;
            });
        }
        createBoard();
    </script>
</body>
</html>
