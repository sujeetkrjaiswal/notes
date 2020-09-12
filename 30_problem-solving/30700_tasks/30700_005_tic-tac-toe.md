# Tic Tac Toe

You can find the code pen below with the implementation

{% embed url="https://codepen.io/sujeetkrjaiswal/pen/OJyKegN" caption="" %}

## File content of code \(For easier access\)

{% tabs %}
{% tab title="index.html" %}
```markup
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <meta http-equiv="X-UA-Compatible" content="ie=edge" />
    <link rel="stylesheet" href="./index.css" />
    <script type="module" src="./index.js"></script>
    <title>Document</title>
  </head>
  <body>
    <main>
      <header id="current-player"></header>
      <table id="game-board">
        <tr class="row">
          <td data-row="0" data-col="0" class="cell"></td>
          <td data-row="0" data-col="1" class="cell"></td>
          <td data-row="0" data-col="2" class="cell"></td>
        </tr>
        <tr class="row">
          <td data-row="1" data-col="0" class="cell"></td>
          <td data-row="1" data-col="1" class="cell"></td>
          <td data-row="1" data-col="2" class="cell"></td>
        </tr>
        <tr class="row">
          <td data-row="2" data-col="0" class="cell"></td>
          <td data-row="2" data-col="1" class="cell"></td>
          <td data-row="2" data-col="2" class="cell"></td>
        </tr>
      </table>

      <footer>
        <button class="btn" id="restart-game">Re start Game</button>
        <button class="btn" id="debug">Debug</button>
      </footer>
    </main>
  </body>
</html>
```
{% endtab %}

{% tab title="index.css" %}
```css
#game-board {
  border-collapse: collapse;
  margin: auto;
}
#current-player {
  height: 50px;
  line-height: 50px;
  font-size: 28px;
  margin-bottom: 24px;
  text-align: center;
}

.cell {
  width: 100px;
  height: 100px;
  border: 2px solid #666666;
  cursor: pointer;
  text-align: center;
  font-size: 56px;
  font-family: fantasy, cursive;
}
#game-board .row:first-child .cell { border-top-width: 0;}
#game-board .row:last-child .cell { border-bottom-width: 0;}
#game-board .row .cell:first-child { border-left-width: 0;}
#game-board .row .cell:last-child { border-right-width: 0;}

.r1 .row:first-child .cell,
.r2 .row:nth-child(2) .cell,
.r3 .row:last-child .cell,
.c1 .row .cell:first-child,
.c2 .row .cell:nth-child(2),
.c3 .row .cell:last-child,
.d1 .row:first-child .cell:first-child,
.d1 .row:nth-child(2) .cell:nth-child(2),
.d1 .row:last-child .cell:last-child,
.d2 .row:first-child .cell:last-child,
.d2 .row:nth-child(2) .cell:nth-child(2),
.d2 .row:last-child .cell:first-child{
  background-color: aquamarine;
  /* border-color: transparent; */
}


footer {
  text-align: center;
  margin-top: 24px;
}
.btn {
  display: inline-block;
  padding: 8px 16px;
  border: none;
  background: #c0392b;
  color: #ecf0f1;
  transition: font-size linear 0.1s;
  font-size: 16px;
  line-height: 18px;
}
.btn:hover {
  font-size: 18px;
}
```
{% endtab %}

{% tab title="index.js" %}
```javascript
import TicTacToe from './tic-tac-toe.js'

const PlayerText = {
  [TicTacToe.PLAYER_ENUM.PLAYER_1]: 'Player 1 (X)',
  [TicTacToe.PLAYER_ENUM.PLAYER_2]: 'Player 2 (O)',
  0: 'Tie'
}

let gameInstance = null
const cells = document.querySelectorAll('.cell')
const currentPlayerElem = document.querySelector('#current-player')
const gameBoard = document.querySelector("#game-board")
document.querySelector('#debug').addEventListener('click', () => {gameInstance.debug()})
document.querySelector('#restart-game').addEventListener('click', restartGame)
gameBoard.addEventListener('click', handleCellClick)

function handleCellClick(e) {
  e = e || window.event
  const target = e.target || e.srcElement
  if(target.tagName.toLowerCase() !== 'td') {
    return
  }
  const rowIdx = Number(target.getAttribute('data-row'))
  const colIdx = Number(target.getAttribute('data-col'))

  try{
    const markSign = gameInstance.activePlayer === -1 ? 'X' : '0'
    gameInstance.mark(rowIdx, colIdx)
    target.textContent = markSign
    const {isGameOver, winner, winnerFlag} = gameInstance.validate()
    if(isGameOver && winner) {
      gameBoard.className = winnerFlag
      currentPlayerElem.textContent = `Game Over: The winner is ${PlayerText[winner]}`
    }else if (isGameOver){
      currentPlayerElem.textContent = `Game Over: It's a tie`
    } else {
      currentPlayerElem.textContent = `Current Player: ${PlayerText[gameInstance.activePlayer]}`
    }
  } catch(err) {
    console.error(err)
  }
}

function restartGame() {
  gameInstance = new TicTacToe(3)
  currentPlayerElem.textContent = `Current Player: ${PlayerText[gameInstance.activePlayer]}`
  gameBoard.className = ''
  cells.forEach(cell => {
    cell.textContent = ''
  })
}
restartGame()
```
{% endtab %}

{% tab title="tic-tac-toe.js" %}
```javascript
export default class TicTacToe {
  static PLAYER_ENUM = Object.freeze({
    PLAYER_1: -1,
    PLAYER_2: 1
  });
  activePlayer = TicTacToe.PLAYER_ENUM.PLAYER_1;

  constructor(size = 3) {
    this._state = Object.seal(
      new Array(3).fill(0).map(_ => Object.seal(new Array(3).fill(0)))
    );
    /**
     * Extra space: 2 for storing meta (all constants for a given instance)
     */
    this._meta = Object.freeze({
      size,
      max_turns: size ** 2
    });
    /**
     * Extra space: (2n + 3) for evaluating winning
     * Updated on every turn.
     * */
    this._processState = Object.seal({
      sumRows: Object.seal(new Array(size).fill(0)),
      sumCols: Object.seal(new Array(size).fill(0)),
      diag1: 0,
      diag2: 0,
      turnCount: 0,
      winner: null,
      winnerFlag: null
    });
  }

  mark(row, col) {
    const size = this._meta.size;
    // validate if any more mark is allowed
    if (this._processState.turnCount >= this._meta.max_turns) {
      throw new Error(`Maximum marks already done.`);
    }
    // validating the boundaries
    if (row < 0 || col < 0 || row >= size || col >= size) {
      throw new Error(
        `Invalid cell. row or col can ve in range (0, ${size -
          1}), both inclusive`
      );
    }
    // validating for already marked
    if (this._state[row][col] !== 0) {
      throw new Error(`Already marked. Please mark on a empty cell`);
    }
    if (this._processState.winner) {
      throw new Error("Game is over");
    }
    // Everything is fine. mark and update the _processState
    const markValue = this.activePlayer;
    this._state[row][col] = markValue;
    this._processState.sumRows[row] += markValue;
    this._processState.sumCols[col] += markValue;
    if (col === row) {
      this._processState.diag1 += markValue;
    }
    if (col === size - row - 1) {
      this._processState.diag2 += markValue;
    }
    this._processState.turnCount += 1;

    this.activePlayer =
      this.activePlayer === TicTacToe.PLAYER_ENUM.PLAYER_1
        ? TicTacToe.PLAYER_ENUM.PLAYER_2
        : TicTacToe.PLAYER_ENUM.PLAYER_1; // Swap the user
  }
  validate() {
    if (this._processState.winner) {
      throw new Error("Already got a winner. cant re-validate");
    }
    if (
      this._validateSum(this._processState.diag1, "d1") ||
      this._validateSum(this._processState.diag2, "d2") ||
      this._processState.sumCols.some((sum, idx) =>
        this._validateSum(sum, `c${idx + 1}`)
      ) ||
      this._processState.sumRows.some((sum, idx) =>
        this._validateSum(sum, `r${idx + 1}`)
      )
    ) {
      // we have a winner: game over
      return {
        isGameOver: true,
        winner: this._processState.winner,
        winnerFlag: this._processState.winnerFlag
      };
    } else if (this._processState.turnCount >= this._meta.max_turns) {
      // we have a tie: game over
      this._processState.winner = 0;
      return { isGameOver: true, winner: 0 };
    }
    // continue with game
    return { isGameOver: false, winner: 0 };
  }
  _validateSum(num, winnerFlag) {
    if (this._meta.size === Math.abs(num)) {
      this._processState.winner = num / this._meta.size;
      this._processState.winnerFlag = winnerFlag
      return true;
    }
    return false;
  }
  debug() {
    const state = [];
    state.push(
      [
        `diag1 sum: ${this._processState.diag1}`,
        ...this._processState.sumCols.map((v, idx) => `col ${idx}: ${v}`)
      ],
      ...this._processState.sumRows.map((rowSum, idx) => [
        `row ${idx}: ${rowSum}`,
        ...this._state[idx]
      ]),
      [`diag2 sum: ${this._processState.diag2}`]
    );
    console.table(state);
    console.log(this._processState);
  }
  isGameOver() {
    return this._processState.winner !== null;
  }
  winner() {
    return this._processState.winner;
  }
}
```
{% endtab %}
{% endtabs %}

