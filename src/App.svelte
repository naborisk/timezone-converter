<script lang="ts">
  import TimeZone from './lib/TimeZone.svelte'
  import Window from './lib/Window.svelte'

  let currentTime = new Date()
  let timeZones = ['loading...']

  interface Window {
    title: string
    id: number
    x: number
    y: number
    data?: any
  }

  let windowList: Window[] = [
    {
      id: 0,
      title: 'Timezone',
      x: 0,
      y: 0
    }
  ]

  let closeWindow = (id: number) => {
    windowList = windowList.filter(item => item.id !== id)
  }

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
    on:click={() => {
      windowList = [
        ...windowList,
        {
          title: 'Timezone',
          x: windowList.length * 20 + 10,
          y: windowList.length * 20 + 10,
          id: crypto.getRandomValues(new Uint32Array(1))[0]
        }
      ]
    }}>add</button
  >

  {#each windowList as w}
    <Window
      close={() => {
        console.log(w.id)
        windowList = windowList.filter(item => {
          return item.id !== w.id
        })
      }}
      on:mousedown={() => {
      //move window to end of array
      windowList = [
        ...windowList.filter(item => item.id !== w.id),
        w
      ]
      }}
      title={w.title}
      bind:x={w.x}
      bind:y={w.y}
    >
      <TimeZone
        dropdownData={timeZones}
        {currentTime}
        title="Converted Time"
        editable={true}
      />
      <!-- {`x: ${w.x} y: ${w.y} id: ${w.id}`} -->
    </Window>
  {/each}
</main>
