<script context="module" lang="ts">
	export const prerender = true;
</script>

<script lang="ts">
	import LineChart from '$lib/LineChart.svelte';
	import GuageSvg from '$lib/GuageSVG.svelte';
	import Animetest from '$lib/Animetest.svelte';
	import { time } from '../stores/weather';
	import { readable, writable } from 'svelte/store';
	import type { Writable } from 'svelte/store';
	$: $time, update();

	//TODO: Set different colors for different curves
	let ydataArray = writable({
		labels: [] as string[],
		datasets: [
			{
				data: [] as number[],
				label: 'First set',
				borderColor: '#f87171',
				tension: 0.1,
				fill: false
			}
		]
	});
	$ydataArray.datasets.push({
		data: [] as number[],
		label: 'Second Set',
		borderColor: '#34d399',
		tension: 0.5,
		fill: false
	});
	let value: Writable<number> = writable(5.2);
	const formatter = new Intl.DateTimeFormat('en', {
		hour12: true,
		hour: 'numeric',
		minute: '2-digit',
		second: '2-digit'
	});
	function update() {
		if ($ydataArray.labels.length > 10) {
			$ydataArray.datasets[0].data.shift();
			$ydataArray.datasets[1].data.shift();
			$ydataArray.labels.shift();
		}
		$ydataArray.labels = [...$ydataArray.labels, formatter.format($time)];
		$value = -10 + Math.floor(Math.random() * 55);
		$ydataArray.datasets[0].data = [...$ydataArray.datasets[0].data, $value];
		$ydataArray.datasets[1].data = [
			...$ydataArray.datasets[1].data,
			-10 + Math.floor(Math.random() * 55)
		];
	}
</script>

<svelte:head>
	<title>Cooling Tests</title>
	<meta name="description" content="Svelte demo app" />
</svelte:head>

<div class="gap-2 p-2 grid grid-cols-1 lg:grid-rows-2 lg:grid-cols-3 lg:gap-4 xl:grid-rows-1">
	<div class="lg:col-span-2 lg:row-span-2 xl:row-span-1">
		<LineChart id="firstChart" title="Temperature Difference" {ydataArray} />
	</div>
	<GuageSvg id="temperature" title="Temperature" {value} units=" &#8451" />
	<!-- <div class="lg:col-span-2">
		<LineChart id="secondChart" title="Temperature and Humidity" xdata={data2} xlabels={labels} />
	</div> -->
	<!-- <GuageSvg id="humidity" title="Humidity" value="-10" units=" %RH" /> -->
</div>
