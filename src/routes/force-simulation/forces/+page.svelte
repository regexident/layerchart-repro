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
		<code>ForceSimulation</code> with <code>d3.forceCenter</code> breaks layout:
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
						center: d3.forceCenter(width / 2, height / 2)
					}}
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
	<h2 class="text-l font-semibold my-1">Suspected cause:</h2>
	<p>
		The breakage is (afaict) caused by <code>ForceSimulation</code> being unaware of
		<code>Chart</code>'s <code>width</code>/<code>height</code>
		and thus not triggering its internal effect (<code>{'$: { simulation = … }'}</code>) when either
		of them changes. As a result the forces never get updated and stick to their initial
		configuration.
	</p>
</main>
