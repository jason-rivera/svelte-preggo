<script>
  import axios from 'axios';
  import { Circle } from 'svelte-loading-spinners';

  let isLoading = false;

  const handleSubmit = async () => {
    try {
      isLoading = true;
      const response = await axios.post('https://netlify-jason-rivera-serverless-functions.netlify.app/.netlify/functions/api/preggo', {
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
	<h3>what can you eat when you're</h3>
	<h1>PREGGO?</h1>
  <div>
    <input on:keypress={handleEnterPress} id="item-input" type="text" />
  </div>
  <br/>

  {#if isLoading}
    <Circle size="35" color="#FF3E00" unit="px" duration="1s"/>
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

#item-input {
  text-align: center;
  padding: 5px;
  font-size: 1.1rem;
}
#ask-btn {
  background: #FF3E00;
  border-radius: 50%;
  padding: 10px;
  cursor: pointer;
  transition: 300ms all;
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