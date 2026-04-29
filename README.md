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
