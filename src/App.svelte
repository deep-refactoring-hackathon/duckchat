<script>
  let message;
  let answer;
  let wasted = false;

	const URL = 'http://localhost:5000/api/v1/chat';

	async function submit() {
		const payload = { message };
		const response = await fetch(URL, {
      method: 'POST',
      mode: 'cors',
			headers: { 'Content-Type': 'application/json' },
			body: JSON.stringify(payload)
    });
    const data = (await response.json()).answers[0]
    answer = data.answer;
    wasted = data.metadata && data.metadata[0] && data.metadata[0].value === 'true';
	}
</script>

<main>
	<div class="duck">
		<img alt="duck" width="100" src="duck.png">
	</div>
	<div>
		<input type="text" bind:value={message}>
		<button on:click="{submit}">Send</button>
	</div>
  <div>
    {answer}
  </div>
  {#if wasted}
    <div class="wasted">Directed By<br>Robert B. Weide</div>
  {/if}
</main>

<style>
	.duck {
		width: 50px;
	}
	main {
		text-align: center;
		padding: 1em;
		max-width: 240px;
		margin: 0 auto;
	}

	h1 {
		color: #ff3e00;
		text-transform: uppercase;
		font-size: 4em;
		font-weight: 100;
	}

	@media (min-width: 640px) {
		main {
			max-width: none;
		}
	}
  .wasted {
    background-color: black;
    color: white;
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    display: flex;
    align-items: center;
    justify-content: center;
  }
</style>