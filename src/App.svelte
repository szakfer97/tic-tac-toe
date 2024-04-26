<script lang="ts">
  let currentPlayer = "X";
  let winner: string | null = null;
  let board = Array(9).fill(null);

  const handleMove = (/** @type {number} */ index: number) => {
    if (!board[index] && !winner) {
      board[index] = currentPlayer;
      winner = calculateWinner();
      currentPlayer = currentPlayer === "X" ? "O" : "X";
    }
  };

  const calculateWinner = () => {
    const lines = [
      [0, 1, 2],
      [3, 4, 5],
      [6, 7, 8],
      [0, 3, 6],
      [1, 4, 7],
      [2, 5, 8],
      [0, 4, 8],
      [2, 4, 6],
    ];

    for (let i = 0; i < lines.length; i++) {
      const [a, b, c] = lines[i];
      if (board[a] && board[a] === board[b] && board[a] === board[c]) {
        return board[a];
      }
    }

    return null;
  };

  const resetGame = () => {
    currentPlayer = "X";
    winner = null;
    board = Array(9).fill(null);
  };
</script>

<main>
  <h1>{winner ? `Winner: ${winner}` : `Next player: ${currentPlayer}`}</h1>

  <div class="board">
    {#each board as cell, index (index)}
      <button class="cell" on:click={() => handleMove(index)}>
        {#if cell !== null}
          {cell}
        {/if}
      </button>
    {/each}
  </div>

  {#if winner}
    <button on:click={resetGame}>Reset</button>
  {/if}
</main>

<style>
  main {
    font-family: sans-serif;
    text-align: center;
  }

  .board {
    display: grid;
    grid-template-columns: repeat(3, 100px);
    grid-template-rows: repeat(3, 100px);
    gap: 5px;
    margin: 20px auto;
    border: 2px solid black;
    width: 320px;
  }

  .cell {
    display: flex;
    justify-content: center;
    align-items: center;
    font-size: 36px;
    background-color: lightblue;
    cursor: pointer;
  }

  button {
    margin-top: 20px;
    padding: 10px 20px;
    font-size: 18px;
    background-color: #4caf50;
    color: white;
    border: none;
    cursor: pointer;
    border-radius: 5px;
  }
</style>
