<script>
	// A boolean prop to control the flip state from the parent.
	export let flipped = false;
</script>

<div class="flip-card-container">
	<div class="flip-card-inner" class:flipped>
		<div class="flip-card-front">
			<slot name="front" />
		</div>
		<div class="flip-card-back">
			<slot name="back" />
		</div>
	</div>
</div>

<style>
	.flip-card-container {
		perspective: 1000px;
		width: 100%;
		height: 100%;
	}

	.flip-card-inner {
		position: relative;
		width: 100%;
		height: 100%;
		transition: transform 0.7s;
		transform-style: preserve-3d;
	}

	.flip-card-inner.flipped {
		transform: rotateY(180deg);
	}

	.flip-card-front,
	.flip-card-back {
		position: absolute;
		width: 100%;
		height: 100%;
		backface-visibility: hidden;
		-webkit-backface-visibility: hidden; /* Safari */
		border-radius: 12px;
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;
		padding: 20px;
		box-shadow: 0 4px 15px rgba(0, 0, 0, 0.1);
	}

	.flip-card-front {
		background: #f7f9fa;
		color: #333;
		border: 1px solid #e0e0e0;
	}

	.flip-card-back {
		background: #2c3e50;
		color: white;
		transform: rotateY(180deg);
	}
</style> 