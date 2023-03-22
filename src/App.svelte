<script>
  import Card from './lib/Card.svelte'

  const urlParams = new URLSearchParams(window.location.search);
  let name = urlParams.get('name') ? urlParams.get('name') : "Jin";
  let slogan = urlParams.get('slogan') ? urlParams.get('slogan') : "Lead Business Analyst";
  let eventDate = urlParams.get('eventdate') ? urlParams.get('eventdate') : "21 May 23";
  let eventTime = urlParams.get('eventtime') ? urlParams.get('eventtime') : "1:30pm";

  let seed = urlParams.get('seed') ? urlParams.get('seed') : "AF000001";
  let additionalData = {};
  try {
    additionalData = urlParams.get('data') ? JSON.parse(urlParams.get('data')) : {};
  } catch (e) {
    console.error(e);
  }
  let qrData = JSON.stringify({
    name, seed, ...additionalData
  });
</script>

<main>
  <Card {name} {slogan} {qrData} {eventDate} {eventTime} />
</main>

<style>
  :root {
    background: rgb(244, 238, 233);
    overflow-y: hidden;
    color: rgb(28, 47, 62);
  }
</style>
