<script lang="ts">
  import anime from "animejs";
  import { onMount } from "svelte";
  import { writable } from "svelte/store";
  export let title: string = "Energy Removed";
  export let value: number = 55;
  export let unit: string = "kJ";

  // onMount(() => {
  //   console.log("Creating anime")

  // });
</script>

<section class="relative min-w-fit m-3 rounded bg-white shadow-lg">
  <div class="rounded-t bg-transparent px-4 py-3">
    <p class="mb-1 text-xs font-semibold uppercase text-black">Overview</p>
    <p class="text-l font-semibold text-black">{title}</p>
    <div class="mt-3 mb-3">
      <p class="text-6xl font-semibold">{value} {unit}</p>
    </div>
  </div>
</section>
