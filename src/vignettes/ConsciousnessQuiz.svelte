<script>
	const questions = [
		"Does it have subjective experiences? (e.g., qualia, emotions)",
		"Can it learn and adapt its behavior based on new information?",
		"Does it exhibit flexible, goal-oriented behavior and planning?",
		"Does it possess self-awareness or a model of itself?",
		"Can it understand and use abstract concepts and language?",
		"Does it demonstrate social awareness and empathy?"
	];
</script>

<div class="quiz-container">
	<div class="header">Human Consciousness Evaluation</div>
	<div class="questions-list">
		{#each questions as question, i}
			<div class="question-item">
				<div class="question-number">Question {i + 1}</div>
				<div class="question-text">{question}</div>
				<div class="response-area">
					<span>Response:</span>
					<div class="line"></div>
				</div>
			</div>
		{/each}
	</div>
	<div class="footer">
		Evaluation Incomplete
	</div>
</div>

<style>
	.quiz-container {
		width: 100%;
		height: 100%;
		background: #fff;
		border: 1px solid #d0e0f0;
		border-radius: 5px;
		padding: 20px;
		box-sizing: border-box;
		font-family: 'Courier New', Courier, monospace;
		color: #333;
		display: flex;
		flex-direction: column;
	}
	.header {
		font-size: 1.2rem;
		font-weight: bold;
		text-align: center;
		padding-bottom: 15px;
		border-bottom: 1px dashed #ccc;
		margin-bottom: 15px;
	}
	.questions-list {
		flex-grow: 1;
		overflow-y: auto;
	}
	.question-item {
		margin-bottom: 15px;
		padding-bottom: 10px;
		border-bottom: 1px solid #eee;
	}
	.question-number {
		font-weight: bold;
		font-size: 0.9rem;
		color: #555;
	}
	.question-text {
		font-size: 1rem;
		margin: 5px 0;
	}
	.response-area {
		display: flex;
		align-items: center;
		margin-top: 10px;
	}
	.response-area span {
		font-style: italic;
		color: #777;
		margin-right: 10px;
	}
	.line {
		flex-grow: 1;
		height: 1px;
		background-color: #bbb;
	}
	.footer {
		text-align: center;
		padding-top: 15px;
		border-top: 1px dashed #ccc;
		font-weight: bold;
		color: #d9534f;
	}
</style> 