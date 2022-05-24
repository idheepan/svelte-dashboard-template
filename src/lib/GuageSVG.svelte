<script lang="ts">
  var svgNS = "http://www.w3.org/2000/svg";
  import type { Writable } from "svelte/store";
  import { onMount, tick } from "svelte";
  import anime from "animejs";
  import type { AnimeInstance } from "animejs";
  export let id: string;
  export let title: string;
  export let value: Writable<number>;
  export let units: string;

  const spacing = 5;
  const radius = 50;
  const angle_min = -90;
  const angle_max = 90;
  const range_min = -10;
  const range_max = 55;
  const center = { x: 0, y: 0 };
  const colors = ["#38bdf8", "#34d399", "#fb923c", "#f87171"];
  //TODO: Roll the number instead of jump.
  let markerEl: HTMLElement;
  let animation: AnimeInstance;
  $: $value, update();

  function update() {
    if (typeof markerEl !== "undefined" && typeof animation !== "undefined") {
      $value = $value.toFixed(2);
      animation.remove();
      animation = anime({
        targets: markerEl,
        rotate: function (element, i) {
          return (
            angle_min +
            ((angle_max - angle_min) / (range_max - range_min)) *
              ($value - range_min)
          );
        },
        easing: "easeInOutSine",
        duration: 1000,
        delay: 250,
      });
    }
  }

  function polarToCartesian(
    centerX: number,
    centerY: number,
    radius: number,
    angleInDegrees: number
  ) {
    var angleInRadians = ((angleInDegrees - 90) * Math.PI) / 180.0;

    return {
      x: centerX + radius * Math.cos(angleInRadians),
      y: centerY + radius * Math.sin(angleInRadians),
    };
  }

  function describeArc(
    x: number,
    y: number,
    startAngle: number,
    endAngle: number
  ) {
    var start = polarToCartesian(x, y, radius, endAngle);
    var end = polarToCartesian(x, y, radius, startAngle);
    var largeArcFlag = endAngle - startAngle <= 180 ? "0" : "1";
    var d = [
      "M",
      start.x,
      start.y,
      "A",
      radius,
      radius,
      0,
      largeArcFlag,
      0,
      end.x,
      end.y,
    ].join(" ");
    return d;
  }

  function createArcSection(
    start_angle: number,
    end_angle: number,
    color: string,
    svg: Element
  ) {
    var arc = document.createElementNS(svgNS, "path"); //to create a circle. for rectangle use "rectangle"
    arc.setAttribute(
      "d",
      describeArc(center.x, center.y, start_angle, end_angle)
    );
    arc.setAttributeNS(null, "fill", "none");
    arc.setAttributeNS(null, "stroke", color);
    arc.setAttributeNS(null, "stroke-width", "10");
    svg.appendChild(arc);
  }

  function createTrack() {
    let num_colors = colors.length;
    let segment_angle = 180 / num_colors;
    let svg = document.getElementById(id)!;
    colors.forEach((color, index) => {
      let start_angle_offset = -90 + spacing / 2;
      let end_angle_offset = -90 - spacing / 2;
      if (index === num_colors - 1) {
        end_angle_offset = -90;
      }
      if (index === 0) {
        start_angle_offset = -90;
      }
      let start_angle = segment_angle * index + start_angle_offset;
      let end_angle = segment_angle * (index + 1) + end_angle_offset;
      createArcSection(start_angle, end_angle, color, svg);
    });
    let markerGroup = document.getElementById("marker")!;
    svg.appendChild(markerGroup);
  }

  onMount(async () => {
    createTrack();
    markerEl = document.getElementById("marker");
    animation = anime({
      targets: markerEl,
      rotate: function (element, i) {
        return $value;
      },
      easing: "easeInOutSine",
      duration: 1000,
      delay: 250,
    });
  });
</script>

<div
  class="flex flex-col relative w-full min-w-0 break-words rounded bg-white shadow-lg items-center"
>
  <div class="mb-0 rounded-t bg-transparent px-4 py-3">
    <div class="flex flex-wrap items-center">
      <div class="relative h-max w-full flex-1 flex-grow">
        <h2 class="text-xl font-semibold text-black">{title}</h2>
      </div>
    </div>
  </div>
  <svg
    class="bg-white"
    {id}
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
      {$value}{units}
    </text>
    <circle
      id="marker"
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
