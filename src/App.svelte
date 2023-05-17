<script>
  import axios from 'axios';
  import { Circle } from 'svelte-loading-spinners';

  let isLoading = false;

  const handleSubmit = async () => {
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
</main>

<style>
  .main-container {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    height: 100vh;
    text-align: center;
    padding: 20px;
    transition: 300ms all;
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
</style>
