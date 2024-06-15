<script lang="ts">
	import * as d3 from 'd3';

	import { ForceSimulation, Chart, Svg, Link, Circle, Group } from 'layerchart';

	import { data } from '../data';

	const root = d3.hierarchy(data);
	const nodes = root.descendants();
	const links = root.links();
</script>

<main class="p-2">
	<h1 class="text-xl font-semibold mb-1">
		<code>ForceSimulation</code> with <code>cloneData</code> breaks layout:
	</h1>
	<div class="h-[600px] p-4 border rounded">
		<Chart data={nodes} let:width let:height>
			<Svg>
				<ForceSimulation
					forces={{
						link: d3
							.forceLink(links)
							.id((d) => d.id)
							.distance(0)
							.strength(1),
						charge: d3.forceManyBody().strength(-50),
						x: d3.forceX(),
						y: d3.forceY()
					}}
					cloneData
					let:nodes
				>
					<Group center>
						{#key nodes}
							{#each links as link}
								<Link data={link} class="stroke-surface-content/50" curve={d3.curveLinear} />
							{/each}
						{/key}

						{#each nodes as node}
							<Circle cx={node.x} cy={node.y} r={3} class="stroke-surface-content" />
						{/each}
					</Group>
				</ForceSimulation>
			</Svg>
		</Chart>
	</div>
</main>
