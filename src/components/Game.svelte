<script>
  import GameItem from "./GameItem.svelte";
  let attempts = 0;
  let n = 32;
  $: gameArray = Array(n);
  let activeIndex = null;
  let activeInterval = setInterval(() => {
    activeIndex = Math.floor(Math.random() * Math.floor(n - 1));
  }, 1000);

  const handleSelect = ({ detail }) => {
    if (n === 1) {
      alert("you win");
      return;
    }
    if (detail == activeIndex) {
      n = n - 1;
      console.log(gameArray);
      return;
    }

    attempts = attempts + 1;
  };
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
  .game {
    text-align: center;
  }
  .attempts {
    padding: 50px 0;
    text-align: center;
    color: tomato;
    font-weight: bolder;
    font-size: 40px;
  }
  .game-body {
    display: flex;
    flex-wrap: wrap;
    flex: 1;
    padding: 30px 50px;
  }
</style>

<main class="container">
  <div class="game">
    <h2 class="game-title">Game Svelte</h2>
    <div class="attempts">
      Attempts
      <span class="att-num">{attempts}</span>
    </div>
    <div class="game-body">
      {#each gameArray as _, i}
        <GameItem {activeIndex} on:select={handleSelect} index={i} />
      {/each}
    </div>
  </div>
</main>
