<script>
  import axios from 'axios';
  import { Circle } from 'svelte-loading-spinners';

  let isLoading = false;

  // $: items = localStorage.getItem('items')
  //   ? JSON.parse(localStorage.getItem('items'))
  //   : [];

  /** TEST: Highlight all text in input text field after clicking on input text field */

  // window.addEventListener('DOMContentLoaded', (event) => {
  //   const itemInput = document.getElementById('item-input');
  //   itemInput.addEventListener('click', () => {
  //     this.focus();
  //     this.select();
  //   });
  // });

  const addToLocalStorage = (itemAnswer) => {
    let items = localStorage.getItem('items')
      ? JSON.parse(localStorage.getItem('items'))
      : [];

    if (items.length > 9) {
      items.shift();
    }

    items.push(
      new Array(document.getElementById('item-input').value, itemAnswer)
    );

    localStorage.setItem('items', JSON.stringify(items));
  };

  const displayLocalStorage = () => {
    const items = localStorage.getItem('items')
      ? JSON.parse(localStorage.getItem('items'))
      : [];

    const itemInput = document.getElementById('item-input');
    const history = document.getElementById('history');
    history.innerHTML = '';

    items.reverse().forEach((item, i) => {
      if (i > 10) return;
      const itemDiv = document.createElement('div');
      itemDiv.addEventListener('click', () => {
        itemInput.value = item[0];

        const answer = document.getElementById('answer');
        answer.innerHTML = item[1];
        closeMenu();
      });
      itemDiv.innerHTML = item[0];
      history.appendChild(itemDiv);
    });
  };

  const handleSubmit = async () => {
    try {
      isLoading = true;
      document.getElementById('answer').innerHTML = '';

      const response = await axios.post(
        'https://lknishac40.execute-api.eu-central-1.amazonaws.com/prod/preggo',
        {
          // const response = await axios.post('https://netlify-jason-rivera-serverless-functions.netlify.app/.netlify/functions/api/preggo',
          item: document.getElementById('item-input').value,
        }
      );

      let answer = response.data.completion.content;

      document.getElementById('answer').innerHTML = answer;
      addToLocalStorage(answer);
    } catch (e) {
      console.error(e);
    }
    isLoading = false;
  };

  const handleEnterPress = (event) => {
    if (event.key === 'Enter') {
      handleSubmit();
    }
  };

  const openMenu = () => {
    displayLocalStorage();
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
    <button id="open-btn" class="open-button" on:click={openMenu}
      >History</button
    >
  </div>
  <div id="side-nav" class="side-nav">
    <button class="close-button" on:click={closeMenu}>Close</button>
    <div id="history" class="history-items" />
  </div>
</main>

<style>
  .main-container {
    position: relative;
    display: flex;
    flex-direction: column;
    align-items: center;

    height: 100vh;
    text-align: center;
    padding: 80px 20px;
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
    position: fixed;
    top: 0;
    right: 0;
  }

  .open-button {
    padding: 15px;
    font-size: 1.5rem;
    cursor: pointer;
    background: red;
    font-weight: bold;
    color: white;
    border: none;
  }

  .side-nav {
    position: fixed;
    overflow: hidden;
    transition: 300ms all;
    background: red;
    top: 0;
    left: 100vw;
    height: 100vh;
    width: 200px;
    display: flex;
    flex-direction: column;
  }

  .close-button {
    align-self: flex-end;
    padding: 15px;
    font-size: 1.2rem;
    cursor: pointer;
    background: red;
    color: black;
    font-weight: bold;
    border: none;
  }

  .history-items {
    display: flex;
    flex-direction: column;
    gap: 20px;
    font-size: 1.5rem;
    font-weight: bold;
    cursor: pointer;
  }
</style>
