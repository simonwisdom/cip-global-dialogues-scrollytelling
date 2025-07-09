<script>
	import { onMount } from 'svelte';

	let aiMessage = "Hello. I can assist you with any questions you have.";
	let humanMessage = "hey uhh,,, whats the best way to cook a sweet potato?";
	let displayedHumanMessage = "";
	let showTypingIndicator = true;
	let aiVisible = false;
	let humanVisible = false;

	function typeMessage(message, speed) {
		let i = 0;
		const interval = setInterval(() => {
			displayedHumanMessage += message.charAt(i);
			i++;
			if (i > message.length) {
				clearInterval(interval);
				showTypingIndicator = false;
			}
		}, speed);
	}

	onMount(() => {
		setTimeout(() => {
			aiVisible = true;
		}, 500);

		setTimeout(() => {
			humanVisible = true;
			setTimeout(() => {
				typeMessage(humanMessage, 100);
			}, 1000);
		}, 1500);
	});
</script>

<div class="container">
	<div class="chat-window">
		<div class="title">AI Assistant</div>
		<div class="messages">
			{#if aiVisible}
				<div class="message ai">{aiMessage}</div>
			{/if}
		</div>
	</div>

	<div class="chat-window">
		<div class="title">Human</div>
		<div class="messages">
			{#if humanVisible}
				<div class="message human">
					{displayedHumanMessage}
					{#if showTypingIndicator}
						<span class="typing-indicator"></span>
					{/if}
				</div>
			{/if}
		</div>
	</div>
</div>

<style>
	.container {
		display: flex;
		justify-content: space-around;
		width: 100%;
		height: 100%;
		padding: 20px;
		box-sizing: border-box;
		font-family: sans-serif;
	}
	.chat-window {
		width: 45%;
		height: 100%;
		border: 1px solid #ccc;
		border-radius: 5px;
		display: flex;
		flex-direction: column;
		background: #fff;
	}
	.title {
		padding: 10px;
		background: #eee;
		border-bottom: 1px solid #ccc;
		font-weight: bold;
		color: #333;
	}
	.messages {
		flex-grow: 1;
		padding: 10px;
		display: flex;
		flex-direction: column;
		justify-content: flex-end;
	}
	.message {
		padding: 8px 12px;
		border-radius: 15px;
		margin-bottom: 5px;
		max-width: 80%;
		line-height: 1.4;
	}
	.ai {
		background: #e1e1e1;
		color: #000;
		align-self: flex-start;
	}
	.human {
		background: #206095;
		color: #fff;
		align-self: flex-start; /* To show it coming from the 'left' */
		min-height: 1.4em; /* To prevent jitter */
	}

	.typing-indicator {
    display: inline-block;
    width: 5px;
    height: 5px;
    background-color: #aaa;
    border-radius: 50%;
    animation: typing 1s infinite;
    vertical-align: middle;
  }
	.typing-indicator::after,
  .typing-indicator::before {
    content: '';
    display: inline-block;
    position: absolute;
    width: 5px;
    height: 5px;
    background-color: #aaa;
    border-radius: 50%;
    animation: typing 1s infinite;
  }
  .typing-indicator::before {
    margin-left: -10px;
    animation-delay: -0.2s;
  }
  .typing-indicator::after {
    margin-left: 10px;
    animation-delay: 0.2s;
  }

  @keyframes typing {
    0%, 100% {
      transform: translateY(0);
    }
    50% {
      transform: translateY(-3px);
    }
  }
</style> 