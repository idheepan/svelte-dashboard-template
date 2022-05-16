<script lang="ts">
	import { writable, type Writable } from 'svelte/store';
	import { onMount } from 'svelte';

	import chartjs from 'chart.js/auto';
	import type { DateTime } from 'luxon';
	import 'chartjs-adapter-luxon';

	export let id: string;
	export let title: string;
	export let datasets: Writable<
		{
			label: string;
			backgroundColor: string;
			borderColor: string;
			data: { x: DateTime; y: number }[];
		}[]
	> = writable([]);
	let ctx: CanvasRenderingContext2D;
	let chartCanvas: HTMLCanvasElement;
	let chart: Chart;

	$: $datasets, update_chart();

	function update_chart() {
		if (typeof chart != 'undefined') {
			console.log('Trying to update line chart');
			chart.data.datasets = $datasets;
			chart.update('none');
		}
	}

	onMount(async () => {
		ctx = chartCanvas.getContext('2d')!;

		chart = new chartjs(ctx, {
			type: 'line',
			data: {
				datasets: $datasets
			},
			options: {
				scales: {
					x: {
						type: 'time'
					}
				}
			}
		});
		//TODO: We need to know the number of datasets on mount. Change it to one where it can be dynamic
		chart.data.datasets?.forEach((dataset) => {
			dataset.pointRadius = 0;
			dataset.tension = 0.1;
		});
	});
</script>

<section class="relative h-full w-full min-w-0 break-words rounded bg-white shadow-lg">
	<div class="mb-0 rounded-t bg-transparent px-4 py-3">
		<div class="flex flex-wrap items-center">
			<div class="relative w-full h-max-w-full flex-1 flex-grow">
				<h6 class="mb-1 text-xs font-semibold uppercase text-black">Overview</h6>
				<h2 class="text-xl font-semibold text-black">{title}</h2>
			</div>
		</div>
	</div>
	<div class="relative m-2 flex">
		<canvas bind:this={chartCanvas} {id} />
	</div>
</section>