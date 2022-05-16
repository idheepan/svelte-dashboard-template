<script lang="ts">
	import { onMount } from 'svelte';
	import type { Writable } from 'svelte/store';
	// library that creates chart objects in page
	import Chart from 'chart.js/auto';
	export let id = 'firstChart';
	export let title: string;
	export let ydataArray: Writable<{
		labels: string[];
		datasets: {
			data: number[];
			label: string;
			borderColor: string;
			tension?: number;
			fill?: boolean;
		}[];
	}>;
	// export let labels: Writable<Array<string>>;

	const colors = ['#38bdf8', '#34d399', '#fb923c', '#f87171'];
	var chart: Chart;
	$: $ydataArray, updateChart();
	//TODO: Initialize this function within onMount after just declaring it outside
	function updateChart() {
		if (typeof chart !== 'undefined') {
			chart.data = $ydataArray;
			chart.update();
			console.log('chart updated');
		} else {
		}
	}

	// init chart
	onMount(async () => {
		var config = {
			type: 'line',
			data: $ydataArray,
			options: {
				maintainAspectRatio: false,
				responsive: true,
				title: {
					display: true,
					text: 'Sales Charts',
					fontColor: 'white'
				},
				legend: {
					labels: {
						fontColor: 'white'
					},
					align: 'end',
					position: 'bottom'
				},
				tooltips: {
					mode: 'index',
					intersect: false
				},
				hover: {
					mode: 'nearest',
					intersect: true
				}
			}
		};
		var ctx = (<HTMLCanvasElement>document.getElementById(id)!).getContext('2d')!;
		chart = new Chart(ctx, config);
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
	<div class="relative m-2 flex h-64 lg:h-96">
		<canvas {id} />
	</div>
</section>
