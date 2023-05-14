<script>
  import Card from './lib/Card.svelte'
  import Game1 from './lib/Game1.svelte'
  import Game2 from './lib/Game2.svelte'
  import Game3 from './lib/Game3.svelte'

  let chaine = 'Welcome to Game SPACE';

  const name = localStorage.getItem('name');
  const scoreGame1 = parseInt(localStorage.getItem('scoreGame1')) || 0;
  const scoreGame2 = parseInt(localStorage.getItem('scoreGame2')) || 0;
  const scoreGame3 = parseInt(localStorage.getItem('scoreGame3')) || 0;
  const score = scoreGame1 + scoreGame2 + scoreGame3;

  let showView1 = true;
  let showView2 = false;

  if (name) {
    chaine = 'Welcome ' + name + ' to Game SPACE' + ',' + ' Your global score is ' + score;
    showView1 = false;
    showView2 = true;
  }


  // function test(cardNumber) {
  //   alert(cardNumber);
  // }


  let viewCard1 = false,
  viewCard2 = false,
  viewCard3 = false;

  const card1 = () => { viewCard1 = true; showView2 = false; }
  const card2 = () => { viewCard2 = true; showView2 = false; }
  const card3 = () => { viewCard3 = true; showView2 = false; }

  function getBack() {
      viewCard1 = false;
      viewCard2 = false;
      viewCard3 = false;
      showView2 = true;
  }

</script>

<main>
  <h1>GameVerse</h1>
  <h2>{chaine.toUpperCase()}!</h2>

  {#if showView1}

  <div>    
    <label for="name">Enter your name:</label>
    <input type="text" id="name" name="name" />

    <button
      on:click={() => {
        const nameInput = document.getElementById('name');
        localStorage.setItem('name', nameInput.value);
        location.reload();
      }}
    > Save </button>
  </div>

  {/if}

  <br> <br> 

  {#if showView2}
  <div class="card">
    <Card cardName={"Pendu"} cardNumber={1} on:card={card1} />
  </div>

  <div class="card">
    <Card cardName={"Trivia"} cardNumber={2} on:card={card2} />
  </div>

  <div class="card">
    <Card cardName={"Speech"} cardNumber={3} on:card={card3} />
  </div>
  {/if}


  {#if viewCard1}
    <Game1 />
    <button on:click={getBack}>Back</button>
  {/if}

  {#if viewCard2}
    <Game2 />
    <button on:click={getBack}>Back</button>
  {/if}

  {#if viewCard3}
    <Game3 />
    <button on:click={getBack}>Back</button>
  {/if} 

</main>


  <h1> Score by game : </h1>
  <p> Game 1 : {scoreGame1} </p>
  <p> Game 2 : {scoreGame2} </p>
  <p> Game 3 : {scoreGame3} </p>


<style>
  .card {
    position: relative;
    display: inline-table;
    width: 300px;
    height: 200px;
    /* background-color: rgba(255, 255, 255, 0.87); */
    border-radius: 10px;
    overflow: hidden;
    cursor: pointer;
    transition: transform 0.3s;
  }

  .card:hover {
    transform: scale(1.05);
  }

  .card img {
    width: 100%;
    height: 100%;
    /* object-fit: cover; */
    /* background-color: rgba(255, 255, 255, 0.87); */

  }

  .card-content {
    position: absolute;
    bottom: 0;
    left: 0;
    right: 0;
    /* padding: 10px; */
    /* background-color: rgba(0, 0, 0, 0.7); */
    color: #fff;
    font-size: 24px;
    text-align: center;
    transition: opacity 0.3s;
    opacity: 0;
  }

  .card:hover .card-content {
    opacity: 1;
  }
</style>
