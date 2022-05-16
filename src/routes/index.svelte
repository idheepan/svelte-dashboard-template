<script context="module" lang="ts">
	export const prerender = true;
</script>

<script lang="ts">
	import LineChart from "$lib/LineChart.svelte";
	import GuageSvg from "$lib/GuageSVG.svelte";
	import DataCard from "$lib/DataCard.svelte";
	import { time } from "../stores/weather";
	import { writable, type Writable } from "svelte/store";
	import { DateTime } from "luxon";
	let datasets: Writable<
		{
			label: string;
			backgroundColor: string;
			borderColor: string;
			data: {
				x: DateTime;
				y: number;
			}[];
		}[]
	> = writable([
		{
			label: "Inlet",
			backgroundColor: "rgb(255, 99, 132)",
			borderColor: "rgb(255, 99, 132)",
			data: [],
		},
		{
			label: "Outlet",
			backgroundColor: "rgb(255, 132, 99)",
			borderColor: "rgb(255, 132, 99)",
			data: [],
		},
	]);
	let energy_removed: Writable<number> = writable(0);

	$: $time, update();
	let value: Writable<number> = writable(5.2);
	const formatter = new Intl.DateTimeFormat("en", {
		hour12: true,
		hour: "numeric",
		minute: "2-digit",
		second: "2-digit",
	});

	let current_time = DateTime.fromISO("2022-04-14T12:00:55");
	let previously_updated_time = current_time.minus({ minutes: 30 });

	async function update() {
		current_time = current_time.plus({ seconds: 25 });
		let request_string: string =
			"http://127.0.0.1:8000/sensor-data/retreive?startts=" +
			previously_updated_time.plus({ milliseconds: 200 }).toString() +
			"&endts=" +
			current_time.toString();
		await fetch(request_string, { method: "GET" })
			.then((response) => {
				if (response.ok) {
					return response.json();
				}
				throw new Error("Something went wrong while fetching data");
			})
			.then((data) => {
				if (data.length > 0) {
					data.forEach((row) => {
						$datasets[row.sensor].data.push({
							x: DateTime.fromISO(row.ts),
							y: row.temperature,
						});
					});

					previously_updated_time = DateTime.fromISO(
						data[data.length - 1].ts
					);
					$datasets = $datasets;
				} else {
					console.log("No data recieved for fetch:", request_string);
				}
			})
			.catch((error) => {
				console.log(error);
				return [];
			});
	}
</script>

<svelte:head>
	<title>Cooling Tests</title>
	<meta name="description" content="Svelte demo app" />
</svelte:head>

<DataCard />
<div
	class="gap-2 p-2 grid grid-cols-1 lg:grid-rows-2 lg:grid-cols-3 lg:gap-4 xl:grid-rows-1"
>
	<div class="lg:col-span-2 lg:row-span-2 xl:row-span-1">
		<LineChart id="firstChart" title="Temperature Difference" {datasets} />
	</div>
	<GuageSvg id="temperature" title="Temperature" {value} units=" &#8451" />
	<!-- <div class="lg:col-span-2">
		<LineChart id="secondChart" title="Temperature and Humidity" xdata={data2} xlabels={labels} />
	</div> -->
	<!-- <GuageSvg id="humidity" title="Humidity" value="-10" units=" %RH" /> -->
</div>
