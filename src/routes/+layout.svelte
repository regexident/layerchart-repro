<script lang="ts">
	import { AppBar, AppLayout, Card, Button, NavItem, Tooltip, settings } from 'svelte-ux';

	import { page } from '$app/stores';
	import '../app.postcss';

	settings({
		classes: {
			AppBar: 'bg-primary text-white shadow-md',
			AppLayout: {
				nav: 'bg-neutral-800 py-2'
			},
			NavItem: {
				root: 'text-sm text-gray-400 pl-6 py-2 hover:text-white hover:bg-gray-300/10 [&:where(.is-active)]:text-sky-400 [&:where(.is-active)]:bg-gray-500/10'
			}
		}
	});
</script>

<AppLayout>
	<svelte:fragment slot="nav">
		<ul>
			<li>
				<NavItem path="/force-simulation/" text="ForceSimulation" currentUrl={$page.url} />
				<ul>
					<li>
						<NavItem
							path="/force-simulation/clone-data"
							text="cloneData (bug)"
							currentUrl={$page.url}
						/>
					</li>
				</ul>
			</li>
		</ul>
	</svelte:fragment>

	<slot />
</AppLayout>

<style lang="postcss">
	ul {
		@apply pl-4 list-disc ml-2;
	}
</style>
