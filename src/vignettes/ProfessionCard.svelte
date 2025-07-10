<script>
	import FlipCard from '../ui/FlipCard.svelte';

	export let profession;
	let flipped = false;
	let selectedFuture = null; // Can be 'human' or 'ai'

	function chooseFuture(future) {
		selectedFuture = future;
		flipped = true;
	}

	function goBack() {
		flipped = false;
	}
</script>

<div class="profession-card-wrapper">
	<FlipCard {flipped}>
		<div slot="front" class="card-content neutral-face">
			<h3 class="profession-name">{profession.name}</h3>
			<div class="button-group">
				<button class="choice-button" on:click|stopPropagation={() => chooseFuture('human')}>Human-Led</button>
				<button class="choice-button" on:click|stopPropagation={() => chooseFuture('ai')}>AI-Driven</button>
			</div>
		</div>
		<div slot="back" class="card-content story-face" class:ai-future={selectedFuture === 'ai'}>
			{#if selectedFuture}
				<div class="story-body">
					<div class="image-container">
						<img class="future-image" src={profession[selectedFuture].image} alt={profession[selectedFuture].title} />
					</div>
					<div class="story-text">
						<h4 class="card-title">{profession[selectedFuture].title}</h4>
						<p class="card-story">{profession[selectedFuture].story}</p>
					</div>
				</div>
			{/if}
			<div class="card-footer">
				<button class="back-button" on:click|stopPropagation={goBack}>&larr; Choose Again</button>
			</div>
		</div>
	</FlipCard>
</div>

<style>
	.profession-card-wrapper {
		height: 520px;
		font-family: 'Helvetica Neue', sans-serif;
	}
	.card-content {
		width: 100%;
		height: 100%;
		display: flex;
		flex-direction: column;
		justify-content: center;
		text-align: center;
		box-sizing: border-box;
	}

	/* Front face styles */
	.neutral-face {
		background: #f0f4f8;
		color: #333;
		padding: 20px;
	}
	.profession-name {
		font-size: 1.8em;
		font-weight: 700;
		margin: 0 0 25px 0;
	}
	.button-group {
		display: flex;
		justify-content: center;
		gap: 15px;
	}
	.choice-button {
		font-size: 1em;
		padding: 10px 20px;
		border-radius: 20px;
		border: 2px solid #3498db;
		background: transparent;
		color: #3498db;
		cursor: pointer;
		transition: all 0.2s;
		font-weight: 600;
	}
	.choice-button:hover {
		background: #3498db;
		color: white;
	}

	/* Back face styles */
	.story-face {
		justify-content: space-between;
		padding: 30px;
	}
	.story-body {
		display: flex;
		flex-direction: row; /* Human default */
		align-items: center;
		gap: 25px;
		flex-grow: 1;
		min-height: 0; /* Fix for flexbox overflow */
	}
	.story-face.ai-future .story-body {
		flex-direction: row-reverse; /* AI inverted */
	}
	.image-container {
		flex-basis: 45%;
		height: 100%;
		display: flex;
		align-items: center;
	}
	.future-image {
		width: 100%;
		height: auto;
		max-height: 100%;
		object-fit: contain;
		display: block;
		border-radius: 8px;
	}
	.story-text {
		flex-basis: 55%;
		text-align: left;
	}
	.card-title {
		font-size: 1.5em;
		margin: 0 0 15px 0;
		font-weight: 600;
		color: #a7c5e2;
		line-height: 1.2;
	}
	.card-story {
		font-size: 1.1em;
		line-height: 1.6;
		margin: 0;
	}
	.card-footer {
		flex-shrink: 0;
		margin-top: 20px;
		text-align: center;
	}
	.back-button {
		font-size: 0.9em;
		background: none;
		border: none;
		color: rgba(255, 255, 255, 0.7);
		cursor: pointer;
		padding: 5px 10px;
	}
	.back-button:hover {
		color: white;
	}
</style> 