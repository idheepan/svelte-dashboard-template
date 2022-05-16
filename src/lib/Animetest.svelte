<script>
	import anime from 'animejs';

	import { onMount } from 'svelte';
	export let value;

	const angle_min = -90;
	const angle_max = 90;
	const range_min = -10;
	const range_max = 55;

	let markerEl;
	let animation;
	$: $value, update();

	function update() {
		if (typeof markerEl !== 'undefined' && typeof animation !== 'undefined') {
			animation.remove();
			animation = anime({
				targets: markerEl,
				rotate: function (element, i) {
					return (
						angle_min + ((angle_max - angle_min) / (range_max - range_min)) * ($value - range_min)
					);
				},
				easing: 'easeInOutSine',
				duration: 1000,
				delay: 250
			});
		}
	}

	onMount(() => {
		markerEl = document.getElementById('AnimeTestMarker');
		animation = anime({
			targets: markerEl,
			rotate: function (element, i) {
				return $value;
			},
			easing: 'easeInOutSine',
			duration: 1000,
			delay: 250
		});
	});
</script>

<div
	class="flex flex-col relative w-full min-w-0 break-words rounded bg-white shadow-lg items-center"
>
	<div class="mb-0 rounded-t bg-transparent px-4 py-3">
		<div class="flex flex-wrap items-center">
			<div class="relative h-max w-full flex-1 flex-grow">
				<h2 class="text-xl font-semibold text-black">Title</h2>
			</div>
		</div>
	</div>
	<svg
		class="bg-white"
		id="svg"
		viewBox="-60 -60 120 70"
		xmlns="http://www.w3.org/2000/svg"
		xmlns:xlink="http://www.w3.org/1999/xlink"
	>
		<text
			id="valueLabel"
			text-anchor="middle"
			dominant-baseline="center"
			fill="#000000"
			font-family="Helvetica, Arial, sans-serif"
		>
			10 C
		</text>
		<circle
			id="AnimeTestMarker"
			class="markerCircle"
			cx="0"
			cy="-55"
			r="4"
			fill="black"
			stroke="white"
			stroke-width="1.5"
		/>
	</svg>
</div>
