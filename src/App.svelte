<script>
  import { fade } from 'svelte/transition';

  let message;
  let answer = "Hi! It's Artem! Can you send me 10$, please?";
  let hideAnswer = false;
  let wasted = false;

	const URL = 'http://localhost:5000/api/v1/chat';

	async function submit() {
    hideAnswer = true;
		const payload = { message };
		const response = await fetch(URL, {
      method: 'POST',
      mode: 'cors',
			headers: { 'Content-Type': 'application/json' },
			body: JSON.stringify(payload)
    });
    const data = (await response.json())
    answer = data.text;
    wasted = data.wasted === true;
    hideAnswer = false;
    setTimeout(() => {
      hideAnswer = true;
      answer = 'I really need your money, please send!';
      setTimeout(() => {
        hideAnswer = false;
      }, 1000);
    }, 2000);
	}
</script>

<main>
  <div class="dialog">
    <div class="duck">
      <img alt="duck" width="100" src="duck.png">
    </div>
    {#if !hideAnswer}
      <div class="duck-answer" transition:fade>
        <div class="line">/</div>
        <div class="box">
          {answer || ''}
        </div>
      </div>
    {/if}
  </div>
  <div class="answer-box">
    <input type="text" bind:value={message}>
    <button on:click="{submit}">Send</button>
    <div class="line-answer">\</div>
  </div>
  <div>

  </div>
  {#if wasted}
    <div class="wasted">Directed By<br>Robert B. Weide</div>
  {/if}
</main>

<style>
	.duck {
		width: 100px;
	}
  .dialog {
    display: flex;
    font-family: 'Press Start 2P', cursive;
    font-size: 0.7em;
  }
  .duck-answer {
    display: flex;
  }
  .line {
    width: 20px;
    margin-top: 10px;
  }
  .answer-box {
    display: flex;
    margin-left: 100px;
  }
  .line-answer {
    margin-left: 20px;
    margin-top: 20px;
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