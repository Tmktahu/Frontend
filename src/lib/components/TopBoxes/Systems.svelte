<script lang="ts">
	import { onMount } from 'svelte';
	import type { System } from '$lib/types/Top/System';

	export let url: string;
	export let count: number = 10;
	export let days: number = 7;
	let systems: System[] = [];

	onMount(async () => {
		const response = await fetch(url);
		systems = await response.json();
	});
</script>

<div class="overflow-x-auto" role="table">
	<table class="table-auto min-w-full bg-semi-transparent bg-background-800 rounded-lg shadow-lg">
		<thead>
			<tr class="bg-darker text-white uppercase text-xs leading-normal">
				<th class="px-2 py-1" scope="col" colspan="3">Top {count} Systems</th>
			</tr>
		</thead>
		<tbody class="text-background-300 text-sm">
			{#each systems as system (system.system_id)}
				<tr
					class="border-b border-background-700 hover:bg-background-600 transition-colors duration-300"
					on:click={(window.location.href = `/system/${system.system_id}`)}
				>
					<td class="px2 py-1">
						<img style="width: 32px; height: 32px;" src={`/map.png`} alt="System: {system.name}" />
					</td>
					<td class="px-2 py-1">
						<div class="flex items-center">
							<div>
								<div class="text-primary-400">{system.name}</div>
							</div>
						</div>
					</td>
					<td class="px-2 py-1">
						<span class="ml-2">{system.count}</span>
					</td>
				</tr>
			{/each}
			<tr>
				<td colspan="4" class="text-center py-2 text-background-400">(Kills over last {days} days)</td>
			</tr>
		</tbody>
	</table>
</div>
