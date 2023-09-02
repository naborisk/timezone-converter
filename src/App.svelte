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

<!-- WIP overlay, only visible in production -->
<!-- {#if import.meta.env.PROD} -->
<!--   <div class="h-screen w-screen absolute backdrop-blur"> -->
<!--     <div class="flex h-screen"> -->
<!--       <div class="mx-auto my-auto text-5xl text-white">WIP</div> -->
<!--     </div> -->
<!--   </div> -->
<!-- {/if} -->

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
