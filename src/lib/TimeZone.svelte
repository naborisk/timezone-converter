<script lang="ts">
  export let dropdownData: string[]
  export let currentTime: Date
  export let title: string = 'TimeZone'
  export let editable: boolean = false

  export let currentTimezone = 'Africa/Abidjan'
  let currentTimeString: string

  $: {
    currentTimeString = currentTime.toLocaleTimeString('en-US', {
      timeZone: currentTimezone
    })
  }
</script>

<div
  class="w-96 h-64 flex flex-col rounded justify-center items-center bg-gray-700 shadow-md gap-2"
>
  <div class="text-xl">
    {title}
  </div>
  <div class="text-xl">
    {currentTimeString}
  </div>
  {#if editable}
    <select
      class="w-64 cursor-pointer rounded bg-gray-500 p-2"
      name="timezone"
      bind:value={currentTimezone}
      id=""
    >
      {#each dropdownData as item}
        <option value={item}>{item}</option>
      {/each}
    </select>
  {:else}
    <div>{currentTimezone}</div>
  {/if}
  <slot />
</div>
