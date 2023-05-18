<script>
  import axios from 'axios';
  import { Circle } from 'svelte-loading-spinners';

  let isLoading = false;

  $: items = localStorage.getItem('items')
    ? JSON.parse(localStorage.getItem('items'))
    : [];

  const addToLocalStorage = () => {
    const items = localStorage.getItem('items')
      ? JSON.parse(localStorage.getItem('items'))
      : [];
    items.push(document.getElementById('item-input').value);
    localStorage.setItem('items', JSON.stringify(items));
    displayLocalStorage();
  };

  const displayLocalStorage = () => {
    const items = localStorage.getItem('items')
      ? JSON.parse(localStorage.getItem('items'))
      : [];
    const history = document.getElementById('history');
    history.innerHTML = '';

    items.reverse().forEach((item, i) => {
      if (i > 10) return;
      const itemDiv = document.createElement('div');
      itemDiv.innerHTML = item;
      history.appendChild(itemDiv);
    });
  };

  const handleSubmit = async () => {
    addToLocalStorage();
    try {
      isLoading = true;
      document.getElementById('answer').innerHTML = '';

      const response = await axios.post(
        'https://lknishac40.execute-api.eu-central-1.amazonaws.com/prod/preggo',
        {
          // const response = await axios.post('https://netlify-jason-rivera-serverless-functions.netlify.app/.netlify/functions/api/preggo', {
          item: document.getElementById('item-input').value,
        }
      );
      document.getElementById('answer').innerHTML =
        response.data.completion.content;
    } catch (e) {
      console.log(e);
    }
    isLoading = false;
  };

  const handleEnterPress = (event) => {
    if (event.key === 'Enter') {
      handleSubmit();
    }
  };

  const openMenu = () => {
    document.getElementById('side-nav').style.left = 'calc(100vw - 200px)';
  };

  const closeMenu = () => {
    document.getElementById('side-nav').style.left = '100vw';
  };
</script>

<main class="main-container">
  <div class="subtitle">what can I have when I'm</div>
  <div class="title">PREGGO?</div>
  <input on:keypress={handleEnterPress} id="item-input" type="text" />

  {#if isLoading}
    <div id="loading-circle">
      <Circle size="75" color="#FF3E00" unit="px" duration="1s" />
    </div>
  {:else}
    <div id="ask-btn" on:click={handleSubmit} on:keypress={handleSubmit}>
      Ask
    </div>
  {/if}

  <div id="answer" class="answer" />
  <div class="nav-menu">
    <div id="open-btn" class="nav-button" on:click={openMenu}>History</div>
  </div>
  <div id="side-nav" class="side-nav">
    <div class="history-items" on:click={closeMenu}>Close</div>
    <div id="history" />
  </div>
</main>

<style>
  .main-container {
    position: relative;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    height: 100vh;
    text-align: center;
    padding: 20px;
    transition: 300ms all;
    overflow-x: hidden;
  }

  .title {
    font-size: 4rem;
    font-weight: 900;
  }

  .subtitle {
    font-size: 1.5rem;
  }

  #item-input {
    text-align: center;
    font-size: 1.5rem;
    padding: 10px;
    font-size: 1.1rem;
    border-radius: 20px;
    border: none;
  }

  input:focus {
    outline: none;
  }
  #ask-btn {
    font-size: 1.4rem;
    background: #ff3e00;
    border-radius: 50%;
    padding: 20px;
    cursor: pointer;
    transition: 300ms all;
    font-weight: bold;
    margin: 30px;
  }

  #loading-circle {
    margin: 30px;
  }

  #ask-btn:hover {
    background: #ff3e00;
    opacity: 0.8;
  }

  #answer {
    text-align: left;
    max-width: 500px;
    padding: 0 20px;
  }

  /** Nav Menu */
  .nav-menu {
    position: absolute;
    top: 0;
    right: 0;
  }

  .nav-button {
    padding: 15px;
    font-size: 1.5rem;
    cursor: pointer;
  }

  .side-nav {
    position: absolute;
    overflow: hidden;
    transition: 300ms all;
    background: #ff3e00;
    top: 0;
    left: 100vw;
    height: 100vh;
    width: 200px;
  }

  .history-items {
    padding: 15px;
    font-size: 1.5rem;
    text-align: right;
    cursor: pointer;
  }
</style>
