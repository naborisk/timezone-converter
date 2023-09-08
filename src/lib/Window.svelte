<script lang="ts">
  import { cubicOut } from 'svelte/easing'
  import { tweened } from 'svelte/motion'
  import { scale } from 'svelte/transition'

  export let close: () => void

  export let title: string = 'Untitled Window'
  export let x: number = 0
  export let y: number = 0

  let moving = false

  // let left = tweened(x, {
  //   duration: 200,
  //   easing: cubicOut
  // })
  //
  // let top = tweened(y, {
  //   duration: 200,
  //   easing: cubicOut
  // })

  let left = x
  let top = y
  //
  // $: {
  //   $left = x
  //   $top = y
  // }
</script>

<div
  role="menuitem"
  tabindex="0"
  on:mousedown
  style={`left: ${x}px; top: ${y}px;`}
  class="flex flex-col absolute shadow-gray-800 shadow-md min-w-[150px] min-h-[200px]"
  transition:scale={{duration: 300, start: 0.9, easing: cubicOut}}
>
  <div
    role="menuitem"
    tabindex="0"
    class="flex pl-2 bg-gray-900 cursor-move select-none"
    on:mousedown={() => (moving = true)}
  >
    {title}
    <button class="ml-auto bg-red-500 px-2" on:click={close}>x</button>
  </div>
  <div class="flex-grow bg-gray-800">
    <slot></slot>
  </div>
</div>

<svelte:window
  on:mouseup={() => (moving = false)}
  on:mousemove={e => {
    if (moving) {
      x += e.movementX
      y += e.movementY
    }
  }}
/>
