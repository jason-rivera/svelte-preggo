<script>
  const handleSubmit = () => {
    console.log('submitting');
    fetch('https://netlify-jason-rivera-serverless-functions.netlify.app/.netlify/functions/api/preggo', {
      method: 'POST',
      item: document.getElementById('item').value
    })
      .then(response => response.json())
      .then(data => {
        document.getElementById('answer').innerHTML = data.answer;
      });
  }
</script>

<main class="main-container">
	<h3>what can you eat when you're</h3>
	<h1>PREGGO?</h1>
  <div>
    <input type="text" />
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