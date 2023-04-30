<script>
	// Lolipop
	import * as d3 from 'd3';
	import { onMount } from 'svelte';
	import gsap from 'gsap';

	export let data;

	let width = 500;
	let height = 500;

	const margin = { top: 50, right: 10, bottom: 50, left: 60 };
// Scales
	$: xScale = d3
		.scaleBand()
		.domain(data.map((d) => d.region))
		.range([margin.left, width - margin.right])
		.padding(0.3);

	$: yScale = d3
		.scaleLinear()
		.domain([0, d3.max(data.map((d) => d.both))])
		.range([height - margin.top, margin.bottom]);
	$: rScale = d3
		.scaleLinear()
		.domain([0, d3.max(data.map((d) => d.both))])
		.range([1, 30]);
// Animations
	let tl = gsap.timeline({
		repeat: -1,
		repeatDelay: 1.5,
		yoyo: true,
		defaults: { ease: 'Back.easeInOut', duration:0.5 }
	});
	onMount(() => {
		tl.from('.circle', {
			attr: { cx: 250 },
			r: 80,
		});
		tl.from('.circle', {
			attr: { cy: 250 },
		} ,"<30%");
		tl.to('.circle', {
			r: 10 ,
		} ,"<");
		tl.from('.rect', {
			height:0,
		});
	});
</script>

<!-- <h1>Bar Chart Transistion1</h1> -->
<div class="chart-container" bind:clientWidth={width}>
	<svg {width} {height}>
		{#each data as d}
		<rect
			class="rect"
			x={xScale(d.region)-1}
			y={yScale(d.both)}
			height={yScale(0) - yScale(d.both)}
			width={2}
			rx={1}
		/>
		<circle class="circle" cx={xScale(d.region)} cy={yScale(d.both)} r={rScale(d.both)} />
		{/each}
	</svg>
</div>

<style>
	.circle {
		fill: #070600;
	}
	.rect {
		fill: #070600;
	}

	svg {
		background-color: #EA526F;
	}
</style>
