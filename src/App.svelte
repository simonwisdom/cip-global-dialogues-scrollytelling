<script>
	// CORE IMPORTS
	import { setContext, onMount } from "svelte";
	import { getMotion, setColors } from "./utils.js";
	import { themes } from "./config.js";
	import ONSHeader from "./layout/ONSHeader.svelte";
	import ONSFooter from "./layout/ONSFooter.svelte";
	import Header from "./layout/Header.svelte";
	import Section from "./layout/Section.svelte";
	import Media from "./layout/Media.svelte";
	import Scroller from "./layout/Scroller.svelte";
	import Filler from "./layout/Filler.svelte";
	import Divider from "./layout/Divider.svelte";
	import Toggle from "./ui/Toggle.svelte";
	import Arrow from "./ui/Arrow.svelte";
	import Em from "./ui/Em.svelte";

	// CHART/VIGNETTE IMPORTS
	import { ScatterChart, BarChart } from "@onsvisual/svelte-charts";
	import SolipsismGraph from "./vignettes/SolipsismGraph.svelte";
	import TypingAnimation from "./vignettes/TypingAnimation.svelte";
	import ImageGallery from "./vignettes/ImageGallery.svelte";
	import ConsciousnessQuiz from "./vignettes/ConsciousnessQuiz.svelte";

	// CORE CONFIG (COLOUR THEMES)
	// Set theme globally (options are 'light', 'dark' or 'lightblue')
	let theme = "light";
	setContext("theme", theme);
	setColors(themes, theme);

	// CONFIG FOR SCROLLER COMPONENTS
	// Config
	const threshold = 0.5;
	// State
	let animation = getMotion(); // Set animation preference depending on browser preference
	let id = {}; // Object to hold visible section IDs of Scroller components
	let idPrev = {}; // Object to keep track of previous IDs, to compare for changes
	onMount(() => {
		idPrev = {...id};
	});

	// VIGNETTE 3: PERFORMATIVE CONSCIOUSNESS STATE
	let chartData = [];
	const baseData = [
		{ x: 1, y: 2, group: 'Early Adopters' },
		{ x: 1.5, y: 2.5, group: 'Early Adopters' },
		{ x: 2, y: 1.8, group: 'Early Adopters' },
	];
	const performativeData = [
		{ x: 4, y: 5, group: 'Performers' },
		{ x: 4.5, y: 6, group: 'Performers' },
		{ x: 5, y: 5.5, group: 'Performers' },
		{ x: 6, y: 7, group: 'Performers' },
		{ x: 6.5, y: 8, group: 'Performers' },
		{ x: 7, y: 7.5, group: 'Performers' },
	];

	// VIGNETTE 4: EMPATHY OLYMPICS STATE
	let empathyData = [];
	const empathyContestants = [
		{ id: 'human-a', name: 'Human A', score: 60, color: '#1d6996' },
		{ id: 'ai-3', name: 'AI Model 3', score: 55, color: '#73af48' },
		{ id: 'human-b', name: 'Human B', score: 40, color: '#1d6996' },
		{ id: 'ai-4', name: 'AI Model 4', score: 85, color: '#73af48' }
	];

	// Actions for Scroller components
	const actions = {
		consciousness: {
			step1: () => {
				chartData = [...baseData];
			},
			step2: () => {
				chartData = [...baseData, ...performativeData.slice(0, 3)];
			},
			step3: () => {
				chartData = [...baseData, ...performativeData];
			}
		},
		empathy: {
			start: () => {
				empathyData = empathyContestants.map(c => ({...c})).sort((a,b) => a.score - b.score);
			},
			scenario1: () => {
				// AI is better at recognizing subtle cues
				let newData = empathyContestants.map(c => ({...c}));
				newData.find(c => c.id === 'ai-4').score = 90;
				newData.find(c => c.id === 'human-a').score = 70;
				empathyData = newData.sort((a, b) => a.score - b.score);
			},
			scenario2: () => {
				// Human shows genuine, unprompted compassion
				let newData = empathyContestants.map(c => ({...c}));
				newData.find(c => c.id === 'human-b').score = 95;
				newData.find(c => c.id === 'ai-4').score = 75;
				empathyData = newData.sort((a, b) => a.score - b.score);
			}
		}
	};

	// Code to run Scroller actions when new caption IDs come into view
	function runActions(codes = []) {
		codes.forEach(code => {
			if (id[code] != idPrev[code]) {
				if (actions[code] && actions[code][id[code]]) {
					actions[code][id[code]]();
				}
				idPrev[code] = id[code];
			}
		});
	}
	$: id && runActions(Object.keys(actions)); // Run above code when 'id' object changes

</script>

<ONSHeader filled={true} center={false} />

<Header bgcolor="#206095" bgfixed={true} theme="dark" center={false} short={true}>
	<h1>Human, or Not?</h1>
	<p class="text-big" style="margin-top: 5px">
		Exploring consciousness in the age of AI.
	</p>
	<p style="margin-top: 20px">
		<Toggle label="Animation {animation ? 'on' : 'off'}" mono={true} bind:checked={animation}/>
	</p>
	<div style="margin-top: 90px;">
		<Arrow color="white" {animation}>Scroll to begin</Arrow>
	</div>
</Header>

<!-- VIGNETTE 1: SOLIPSISM -->
<Section>
	<h2>The Rise of Solipsism</h2>
	<p>
		Loads of solipsism. AI companies incentivised to not allow consciousness and spread doubt about this. Lots of people doubting others exist.
	</p>
	<div class="interactive-placeholder" style="height: 400px; background: #282c34; border-radius: 5px;">
		<SolipsismGraph />
	</div>
</Section>

<Divider />

<!-- VIGNETTE 2: INEFFICIENCY -->
<Section>
	<h2>The Performance of Inefficiency</h2>
	<p>
		People trying to appear slower / dumbing down in order to be more obviously human. This would be wild in eg dating or medicine.
	</p>
	<div class="interactive-placeholder" style="height: 200px; background: #F5F5DC; border-radius: 5px;">
		<TypingAnimation />
	</div>
</Section>

<Divider />

<!-- VIGNETTE 3: PERFORMATIVE CONSCIOUSNESS -->
<Scroller {threshold} bind:id={id['consciousness']} splitscreen={true}>
	<div slot="background">
		<figure class="height-full" style="background: #f0f0f0;">
			<div class="chart-container">
				<ScatterChart
					data={chartData}
					xKey="x" yKey="y" zKey="group"
					idKey="id"
					r={[5, 15]}
					xDomain={[0, 10]} yDomain={[0, 10]}
					xLabel="Depth of Inquiry" yLabel="Perceived Consciousness"
					height="calc(100vh - 150px)"
					{animation}
				/>
			</div>
		</figure>
	</div>
	<div slot="foreground">
		<section data-id="step1">
			<div class="col-medium">
				<h2>Performative Consciousness</h2>
				<p>
					Initially, the link between asking deep questions and being seen as conscious is weak and affects only a few.
				</p>
			</div>
		</section>
		<section data-id="step2">
			<div class="col-medium">
				<p>
					But as learning, adaptation, and asking deep questions become known markers of consciousness, more people start exhibiting these behaviors.
				</p>
			</div>
		</section>
		<section data-id="step3">
			<div class="col-medium">
				<p>
					Eventually, it becomes a performative act. A clear trend emerges: the deeper the (performed) inquiry, the higher the perceived consciousness.
				</p>
			</div>
		</section>
	</div>
</Scroller>


<Divider />

<!-- VIGNETTE 4: EMPATHY OLYMPICS -->
<Scroller {threshold} bind:id={id['empathy']} splitscreen={true}>
	<div slot="background">
		<figure class="height-full" style="background: #FFC0CB;">
			<div class="chart-container">
				<BarChart
					data={empathyData}
					xKey="score" yKey="name"
					sort="ascending"
					xDomain={[0, 100]}
					xLabel="Empathy Score"
					height={350} padding={{top: 20, bottom: 30, left: 100, right: 20}}
					{animation}
					color={d => d.color}
				/>
			</div>
		</figure>
	</div>
	<div slot="foreground">
		<section data-id="start">
			<div class="col-medium">
				<h2>The Empathy Olympics</h2>
				<p>
					In a world seeking uniquely human traits, empathy becomes a competitive sport. Humans and AIs are pitted against each other.
				</p>
			</div>
		</section>
		<section data-id="scenario1">
			<div class="col-medium">
				<p>
					In the first test, responding to simulated emotional distress, the AI's speed and logical assessment give it an edge.
				</p>
			</div>
		</section>
		<section data-id="scenario2">
			<div class="col-medium">
				<p>
					But in a surprise real-world event, a human contestant's genuine, unprompted act of compassion wins the day, highlighting a quality the AIs could only simulate.
				</p>
			</div>
		</section>
	</div>
</Scroller>

<Divider />

<!-- VIGNETTE 5: NEO-ROMANTICISM -->
<Section>
	<h2>Neo-Romanticism</h2>
	<p>
		Or we all get really romantic because it's the only thing we all agree we can't delegate to AI
	</p>
	<div class="interactive-placeholder" style="height: 400px; background: #8B0000; border-radius: 5px;">
		<ImageGallery />
	</div>
</Section>

<Divider />

<!-- VIGNETTE 6: HUMAN CONSCIOUSNESS TEST -->
<Section>
	<h2>The Human Consciousness Test</h2>
	<p>
		Survey answers show 6 areas that people are thinking about how to know if an AI is conscious. What if we ask humans these same questoins - would they be regarded as conscious by these same tests?
	</p>
	<div class="interactive-placeholder" style="height: 400px; background: #E6F0FF; border-radius: 5px;">
		<ConsciousnessQuiz />
	</div>
</Section>


<ONSFooter />

<style>
	/* Styles specific to elements within the demo */
	.interactive-placeholder {
		display: flex;
		align-items: center;
		justify-content: center;
		color: #aaa;
		font-style: italic;
	}
	.chart-container {
		width: 100%;
		height: 100%;
		display: flex;
		align-items: center;
		justify-content: center;
		background: white;
		border-radius: 5px;
	}
</style>
