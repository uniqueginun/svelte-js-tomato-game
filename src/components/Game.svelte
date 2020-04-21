<script>
  import GameItem from "./GameItem.svelte";
  import { onDestroy } from "svelte";
  let attempts = 0;
  let n = 32;
  $: gameArray = Array(n);
  let activeIndex = null;
  let start = false;
  let level = 1000;

  const handleSelect = ({ detail }) => {
    if (n === 1) {
      alert("you win");
      finishGame();
      return;
    }
    if (detail == activeIndex) {
      n = n - 1;
      return;
    }

    attempts = attempts + 1;
  };

  const startGame = () => {
    let activeInterval = setInterval(() => {
      activeIndex = Math.floor(Math.random() * Math.floor(n - 1));
    }, level);
    start = true;
  };

  function finishGame() {
    activeIndex = null;
    n = 32;
    attempts = 0;
    start = false;
    learInterval(activeInterval);
  }

  onDestroy(() => {
    clearInterval(activeInterval);
  });
</script>

<style>
  .container {
    display: flex;
    flex-direction: column;
    justify-content: flex-start;
    align-items: center;
    padding-top: 3rem;
    background-color: azure;
    border-radius: 5px;
    width: 700px;
    height: 1024px;
  }
  .game-title {
    font-size: 40px;
  }
  .game {
    text-align: center;
  }
  .attempts {
    padding: 50px 0;
    text-align: center;
    color: tomato;
    font-weight: bolder;
    font-size: 30px;
  }
  .game-body {
    display: flex;
    flex-wrap: wrap;
    flex: 1;
    padding: 30px 50px;
  }
  .diff {
    display: flex;
    align-items: flex-start;
    flex-direction: column;
  }
  button {
    background-color: forestgreen;
    outline: none;
    color: aliceblue;
    padding: 13px 18px;
    border-radius: 5px;
    cursor: pointer;
    margin-top: 20px;
  }
</style>

<main class="container">
  <div class="game">
    <h2 class="game-title">Tomato Game</h2>
    <div class="attempts">
      failed attempts
      <span class="att-num">{attempts}</span>
    </div>
    <div class="game-body">
      {#if !start}
        <div class="diff">
          <label>Select difficulty level</label>
          <br />
          <label>
            <input type="radio" value={1000} bind:group={level} />
            Medium
          </label>
          <label>
            <input type="radio" value={500} bind:group={level} />
            Hard
          </label>
          <label>
            <input type="radio" value={250} bind:group={level} />
            Extream
          </label>
          <button class="start-btn" on:click|once={startGame}>
            START GAME
          </button>
        </div>
      {:else}
        {#each gameArray as _, i}
          <GameItem {activeIndex} on:select={handleSelect} index={i} />
        {/each}
      {/if}

    </div>
    {#if start}
      <button
        class="start-btn"
        style="background-color: red;"
        on:click={finishGame}>
        Too hard
      </button>
    {/if}
  </div>
</main>
