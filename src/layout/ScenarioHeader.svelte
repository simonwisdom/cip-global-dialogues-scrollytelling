<script>
	import { themes } from '../config.js';
	import { getContext } from 'svelte';

	export let theme = getContext('theme');
	export let number = 1;
	export let title = '';
	export let subtitle = '';
	export let color = themes[theme]['text'];
</script>

<div class="scenario-header" style="color: {color}; background-color: {themes[theme]['background']}">
	<div class="col-medium">
		<div class="scenario-number">Scenario {number}</div>
		<h2 class="scenario-title">{title}</h2>
		{#if subtitle}
			<p class="scenario-subtitle">{subtitle}</p>
		{/if}
	</div>
</div>

<style>
	.scenario-header {
		padding: 60px 0 40px 0;
		text-align: center;
		border-top: 4px solid currentColor;
		margin-top: 0;
	}

	.scenario-number {
		font-size: 0.9em;
		text-transform: uppercase;
		letter-spacing: 0.1em;
		opacity: 0.7;
		margin-bottom: 10px;
		font-weight: 600;
	}

	.scenario-title {
		font-size: 2.5em;
		margin: 0 0 10px 0;
		font-weight: 700;
	}

	.scenario-subtitle {
		font-size: 1.2em;
		opacity: 0.8;
		margin: 0;
		max-width: 600px;
		margin: 0 auto;
	}

	@media (max-width: 768px) {
		.scenario-title {
			font-size: 2em;
		}
		.scenario-subtitle {
			font-size: 1.1em;
		}
	}
</style>