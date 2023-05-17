<script>
  import axios from 'axios';

  const handleSubmit = async () => {
    const response = await axios.post('https://netlify-jason-rivera-serverless-functions.netlify.app/.netlify/functions/api/preggo', {
      item: document.getElementById('item-input').value,
    });
    
    document.getElementById('answer').innerHTML = response.data.completion.content;
  }


  // const handleSubmit = async () => {
  //   console.log('submitting');
  //   console.log(document.getElementById('item-input').value);
  //   fetch('https://netlify-jason-rivera-serverless-functions.netlify.app/.netlify/functions/api/preggo', {
  //     method: 'POST',
  //     body: JSON.stringify({ item: document.getElementById('item-input').value }),
  //   })
  //     .then(response => response.json())
  //     .then(data => {
  //       document.getElementById('answer').innerHTML = data.completion.content;
  //     });
  // };
</script>

<main class="main-container">
	<h3>what can you eat when you're</h3>
	<h1>PREGGO?</h1>
  <div>
    <input id="item-input" type="text" />
    <button on:click={handleSubmit}>Submit</button>
  </div>
  <br/>
  <h4>Answer:</h4>
  <div id="answer"></div>
</main>

<style>

.main-container {
  text-align: center;
  display: flex;
  flex-direction: column;
  align-items: center;
  padding: 20px;
}
</style>