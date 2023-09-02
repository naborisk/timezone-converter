<script lang="ts">
  export let dropdownData: string[]
  export let currentTime: Date
  export let title: string = 'TimeZone'
  export let editable: boolean = false

  export let currentTimezone = 'Africa/Abidjan'
  let currentTimeString: string

  let filteredTimezones: string[] = []

  let showDropdown = false

  const filterTimezone = () => {
    showDropdown = true
    return (filteredTimezones =
      currentTimezone !== ''
        ? dropdownData.filter(item => {
            return item.toLowerCase().includes(currentTimezone.toLowerCase())
          })
        : [])
  }

  // update current time
  $: {
    if (dropdownData.includes(currentTimezone)) {
      currentTimeString = currentTime.toLocaleTimeString('en-US', {
        timeZone: currentTimezone
      })
    } else {
      currentTimeString = currentTime.toLocaleTimeString('en-US')
    }
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
    <div class="flex flex-col gap-2 relative">
      <input
        class="bg-gray-500 w-64 p-2 rounded hover:bg-gray-600 focus:bg-gray-600 transition outline-none"
        bind:value={currentTimezone}
        on:input={filterTimezone}
      />
      {#if showDropdown}
        <div
          class="bg-gray-500 absolute max-h-60 top-12 rounded flex flex-col overflow-auto scrollbar-none"
        >
          {#each filteredTimezones as t}
            <button
              class="p-2 w-64 cursor-pointer hover:bg-gray-600 transition"
              on:click={() => {
                console.log('hey')
                currentTimezone = t
                showDropdown = false
              }}>{t}</button
            >
          {/each}
        </div>
      {/if}
    </div>
  {:else}
    <div class="text-lg">{currentTimezone}</div>
  {/if}
  <slot />
</div>
