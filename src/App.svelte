<script lang="ts">
  import TimeZone from './lib/TimeZone.svelte'
  import Window from './lib/Window.svelte'

  let showDraggable = false

  let currentTime = new Date()
  let timeZones = ['loading...']

  interface Window {
    title: string
    x: number
    y: number
  }

  let windowList: Window[] = [
    {
      title: 'Timezone',
      x: 0,
      y: 0
    }
  ]

  ;(async () => {
    timeZones = await fetch('timezones.json').then(res => res.json())
  })()

  setInterval(() => {
    currentTime = new Date()
  }, 1000)
</script>

<main
  class="bg-sky-900 text-white h-[100dvh] flex flex-col items-center justify-center"
>
  <TimeZone
    dropdownData={timeZones}
    {currentTime}
    title="Your Current Time"
    currentTimezone={Intl.DateTimeFormat().resolvedOptions().timeZone}
  />
  <div class="text-4xl">↓</div>
  <TimeZone
    dropdownData={timeZones}
    {currentTime}
    title="Converted Time"
    editable={true}
  />

  <button
    class="mt-5 bg-sky-800 px-3 py-1 rounded"
    on:click={() => (showDraggable = !showDraggable)}>toggle</button
  >

  <button
    class="mt-5 bg-sky-800 px-3 py-1 rounded"
    on:click={() => {
      windowList = [
        ...windowList,
        {
          title: 'Timezone',
          x: windowList.at(-1).x + 20,
          y: windowList.at(-1).y + 20
        }
      ]
    }}>add</button>

  {#each windowList as window}
    <Window
      close={() => (windowList = windowList.filter(item => item !== window))}
      title={window.title}
    >
      <TimeZone
        dropdownData={timeZones}
        {currentTime}
        title="Converted Time"
        editable={true}
      />
    </Window>
  {/each}

  {#if showDraggable}
    <Window
      close={() => (showDraggable = false)}
      title="Timezone"
    >
      <TimeZone
        dropdownData={timeZones}
        {currentTime}
        title="Converted Time"
        editable={true}
      />
    </Window>
  {/if}
</main>
