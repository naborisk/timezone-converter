<script lang="ts">
  import TimeZone from './lib/TimeZone.svelte'

  let currentTime = new Date()
  let timeZones = ['loading...']

  ;(async () => {
    timeZones = await fetch('http://worldtimeapi.org/api/timezone').then(res =>
      res.json()
    )
  })()

  let result: any = {
    timezone: 'Africa/Abidjan'
  }

  let checkLocation = () =>
    fetch('http://ip-api.com/json/')
      .then(res => res.json())
      .then(res => {
        console.log(res)
        result = res
      })

  $: setInterval(() => {
    currentTime = new Date()
  }, 1000)
</script>

{timeZones}

<!-- WIP overlay, only visible in production -->
{#if import.meta.env.PROD}
  <div class="h-screen w-screen absolute backdrop-blur">
    <div class="flex h-screen">
      <div class="mx-auto my-auto text-5xl text-white">WIP</div>
    </div>
  </div>
{/if}

<main
  class="bg-sky-900 text-white h-screen flex flex-col items-center justify-center"
>
  <TimeZone dropdownData={timeZones} {currentTime} title="Current Time">
    <button class="rounded bg-blue-500 p-2" on:click={checkLocation}>
      Get Current Location
    </button>
  </TimeZone>
  <div class="text-4xl">↓</div>
  <TimeZone
    dropdownData={timeZones}
    {currentTime}
    title="Converted Time"
    timeOffset={1}
  />
</main>
