<script lang="ts">
  import TimeZone from './lib/TimeZone.svelte'

  let currentTime = new Date()
  let timeZones = ['loading...']

  ;(async () => {
    timeZones = await fetch('timezones.json').then(res =>
      res.json()
    )
  })()

  $: setInterval(() => {
    currentTime = new Date()
  }, 1000)
</script>

<main
  class="bg-sky-900 text-white h-screen flex flex-col items-center justify-center"
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
</main>
