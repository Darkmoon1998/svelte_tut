<script>
	import * as d3 from 'd3';
	let name = '';
	import { browser } from '$app/env';
	const url =
		'https://gist.githubusercontent.com/netj/8836201/raw/6f9306ad21398ea43cba4f7d537619d0e07d5ae3/iris.csv';

	let data = [];
	fetch(url).then(async (response) => {
		let body = await response.text();
		body = body.split('\n');
		for (let i = 1; i < body.length; i++) {
			let row = body[i].split(',');
			data.push([parseFloat(row[0]), parseFloat(row[1])]);
		}
		data = data;
		let max_data = 0;
		data.forEach((dd) => dd.forEach((d) => (max_data = Math.max(max_data, Math.abs(d)))));

		let x = d3.scaleLinear().domain([-max_data, max_data]).range([0, size.width]);
		let y = d3.scaleLinear().domain([max_data, -max_data]).range([0, size.width]);
		console.log(max_data, data, x(1, 1));

		let dataplotly = {};
		dataplotly.x = data.map((d) => d[0]);
		dataplotly.y = data.map((d) => d[1]);
		dataplotly.mode = 'markers';
		dataplotly.type = 'scatter';
		if (Plotly) new Plotly.newPlot(plot, [dataplotly]);
	});
	let size = { width: 300, height: 300 };
	let x = () => {};
	let y = () => {};
	let plot;
</script>

<svelte:head>
	<script src="https://cdn.plot.ly/plotly-latest.min.js" type="text/javascript"></script>
</svelte:head>

<div>
	<svg width={size.width} height={size.height}>
		{#each data as xy}
			<circle cx={x(xy[0])} cy={y(xy[1])} r="5" fill="red" />
		{/each}
	</svg>
</div>

<div bind:this={plot} />
