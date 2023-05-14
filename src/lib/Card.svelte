<script>
    import { createEventDispatcher } from 'svelte';
  
    const dispatch = createEventDispatcher();
  
    export let cardNumber;
    export let cardName;
  
    const handleClick = () => {
      dispatch('card');
    };
  </script>
  
  <div class="card" on:click={handleClick}>
    <img src="../src/assets/game{cardNumber}.png" alt="Card Image" />
    <div class="card-content">
      <p>{cardName} Game</p>
    </div>
  </div>


  <style>
    .card {
      position: relative;
      width: 300px;
      height: 300px;
      padding: 24px;
      background-color: rgba(255, 255, 255, 0.87);
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
      font-size: 22px;
      font-weight: 600;
      color: rgba(0, 0, 0, 0.9);
      /* background-color: rgba(255, 255, 255, 0.87); */
      text-align: center;
      line-height: 0.2em;
      padding: 0;
    }
  
    .card:hover .card-content {
      opacity: 1;
    }
  </style>
  