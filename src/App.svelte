<script>
  import axios from 'axios';
  import { Circle } from 'svelte-loading-spinners';

  let isLoading = false;

  const handleSubmit = async () => {
    try {
      isLoading = true;
      document.getElementById('answer').innerHTML = '';
      
      const response = await axios.post('https://lknishac40.execute-api.eu-central-1.amazonaws.com/prod/preggo', {
      // const response = await axios.post('https://netlify-jason-rivera-serverless-functions.netlify.app/.netlify/functions/api/preggo', {
        item: document.getElementById('item-input').value,
      });
      document.getElementById('answer').innerHTML = response.data.completion.content;
    } catch (e) {
      console.log(e);
    }
    isLoading = false;
  }

  const handleEnterPress = (event) => {
    if (event.key === 'Enter') {
      handleSubmit();
    }
  }

</script>

<main class="main-container">
	<div class="subtitle">what can you eat when you're</div>
	<div class="title">PREGGO?</div>
  <input on:keypress={handleEnterPress} id="item-input" type="text" />
  <br/>

  {#if isLoading}
    <Circle size="75" color="#FF3E00" unit="px" duration="1s"/>
  {:else}
    <div id="ask-btn" on:click={handleSubmit} on:keypress={handleSubmit}>Ask</div>
  {/if}

  <div id="answer" class="answer"></div>
</main>

<style>

.main-container {
  margin-top: 100px;
  text-align: center;
  display: flex;
  flex-direction: column;
  align-items: center;
  padding: 20px;
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

textarea:focus, input:focus{
  outline: none;
}
#ask-btn {
  font-size: 1.4rem;
  background: #FF3E00;
  border-radius: 50%;
  padding: 20px;
  cursor: pointer;
  transition: 300ms all;
  font-weight: bold;
}

#ask-btn:hover {
  background: #FF3E00;
  opacity: 0.8;
}

.answer {
  padding-top: 20px;
  text-align: left;
  max-width: 500px;
}
</style>