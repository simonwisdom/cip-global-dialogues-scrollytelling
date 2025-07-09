<script>
  import { onMount } from 'svelte';
  import * as d3 from 'd3';

  let svgElement;
  let width;
  let height;

  const numNodes = 50;
  const nodes = Array.from({ length: numNodes }, (_, i) => ({ id: i }));
  const links = d3.pairs(nodes.map(n => n.id)).filter(() => Math.random() > 0.85).map(([source, target]) => ({ source, target }));

  onMount(() => {
    const container = svgElement.parentElement;
    width = container.clientWidth;
    height = container.clientHeight;

    const svg = d3.select(svgElement)
      .attr('width', width)
      .attr('height', height);

    const simulation = d3.forceSimulation(nodes)
      .force('link', d3.forceLink(links).id(d => d.id).distance(40))
      .force('charge', d3.forceManyBody().strength(-100))
      .force('center', d3.forceCenter(width / 2, height / 2));

    const link = svg.append('g')
      .attr('class', 'links')
      .selectAll('line')
      .data(links)
      .enter().append('line');

    const node = svg.append('g')
      .attr('class', 'nodes')
      .selectAll('circle')
      .data(nodes)
      .enter().append('circle')
      .attr('r', 5);

    simulation.on('tick', () => {
      link
        .attr('x1', d => d.source.x)
        .attr('y1', d => d.source.y)
        .attr('x2', d => d.target.x)
        .attr('y2', d => d.target.y);

      node
        .attr('cx', d => d.x)
        .attr('cy', d => d.y);
    });

		// Add a function to break links over time
		const breakLinksInterval = setInterval(() => {
			if (links.length > 5) {
				links.pop();
				simulation.force('link').links(links);
				simulation.alpha(0.3).restart();

				// Update link elements
				svg.selectAll('.links line')
					.data(links)
					.exit().remove();
			} else {
				clearInterval(breakLinksInterval);
			}
		}, 1000);

		return () => {
			clearInterval(breakLinksInterval);
			simulation.stop();
		}
  });
</script>

<div class="graph-container">
  <svg bind:this={svgElement}></svg>
</div>

<style>
  .graph-container {
    width: 100%;
    height: 100%;
  }
  :global(svg .links line) {
    stroke: #999;
    stroke-opacity: 0.6;
  }
  :global(svg .nodes circle) {
    stroke: #fff;
    stroke-width: 1.5px;
		fill: #555;
  }
</style> 