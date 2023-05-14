<script>
  import { onMount } from 'svelte';

  // const words = ['javascript', 'svelte', 'programming', 'jspquoidautre'];
  // const words = getRandomWords();
  const maxAttempts = 8;

  let word = '';
  let hiddenWord = [];
  let attempts = 0;
  let scoreGame1 = 0;
  let guess = '';
  let gameState = 'playing';

  async function initializeGame () {
    const words = await getRandomWords(); 
    word = words[Math.floor(Math.random() * words.length)].toUpperCase();
    hiddenWord = Array.from(word, () => '_');
    attempts = 0;
    guess = '';
    gameState = 'playing';
  };
  
  async function getRandomWords() {
    return fetch('https://trouve-mot.fr/api/random/10')
      .then(res => res.json())
      .then(data => {
        console.log(Array.from(data.map(d => d.name)));
        return Array.from(data.map(d => d.name));
      })

  }
  const checkGuess = () => {
    guess = guess.toUpperCase();
    if (guess.length === 1) {
      const indices = [];
      for (let i = 0; i < word.length; i++) {
        if (word[i] === guess) {
          indices.push(i);
        }
      }
      if (indices.length === 0) {
        attempts++;
      } else {
        indices.forEach((index) => {
          hiddenWord[index] = guess;
        });
      }
      if (hiddenWord.join('') === word) {
        gameState = 'win';
        scoreGame1 += 100;
        localStorage.setItem('scoreGame1', scoreGame1);
      } else if (attempts === maxAttempts) {
        gameState = 'lose';
      }
      guess = '';
    }
  };

  onMount(() => {
    scoreGame1 = parseInt(localStorage.getItem('scoreGame1')) || 0;
    initializeGame();
  });
</script>

<main>
      

  {#if gameState === 'playing'}
    <h1>Pendu Game</h1>
    <h2>Score: {scoreGame1}</h2>
    <h3>attempts remaining : <span id="attempts-count">{maxAttempts - attempts}</span></h3>
    <h3>word : <span id="secret-word">{hiddenWord.join(' ')}</span></h3>
    <p id="input-title">Enter a letter</p>
    <input bind:value={guess} on:input={checkGuess} placeholder="A, B, C, ..." />

  {:else if gameState === 'win'}
    <h1>You win!</h1>
    <h2>Score: {scoreGame1}</h2>
    <button on:click={initializeGame}>Play Again</button>
  {:else if gameState === 'lose'}
    <h1>You lose!</h1>
    <h2>Score: {scoreGame1}</h2>
    <p>The word was: {word}</p>
    <button on:click={initializeGame}>Play Again</button>
  {/if}
</main>

<style>
  h3 {
    font-weight: 500;
  }
  #secret-word {
    margin-left: 16px;
    font-size: 3em;
  }
  main {
    text-align: center;
    margin-top: 50px;
  }

  #input-title {
    font-size: 18px;
    font-weight:600;
    margin: 40px 0 0 0;
    font-size: 28px;
    font-weight: 100;
    text-transform: lowercase;
    /* text-decoration: underline; */
  }
  input {
    /* margin-top: 20px; */
    margin: 8px 0 64px 0;
    padding: 5px;
    background-color: rgb(251, 251, 251, 1);
    color: #333;
    border: 1px solid #333;
    line-height: 100%;
    width: 3em;
    font-size: 3em;
  }
  button {
    margin-top: 48px;
    padding: 10px 20px;
    background-color: rgb(251, 251, 251, 1);
    color: #333;
    border : 2px solid #333;
    /* border: none; */
    cursor: pointer;
  }
  #attempts-count {
    color:rgba(240,10,10,1);
    font-size: 1.8em;
    font-weight: 600;
    font-family: monospace;
  }
</style>

