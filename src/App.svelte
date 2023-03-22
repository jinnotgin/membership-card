<script>
  import Card from './lib/Card.svelte'

  const urlParams = new URLSearchParams(window.location.search);
  let name = urlParams.get('name') ? urlParams.get('name') : "Jin";
  let slogan = urlParams.get('slogan') ? urlParams.get('slogan') : "Consultant, Corporate Strategy";
  let footer1 = urlParams.get('footer1') ? urlParams.get('footer1') : "21 May 23";
  let footer2 = urlParams.get('footer2') ? urlParams.get('footer2') : "1:30pm";

  let seed = urlParams.get('seed') ? urlParams.get('seed') : "AF000001";
  let data = {};
  try {
    data = urlParams.get('data') ? JSON.parse(urlParams.get('data')) : {};
  } catch (e) {
    console.error(e);
  }
  let qrData = JSON.stringify({
    name, seed, ...data
  });
</script>

<main>
  <Card {name} {slogan} {qrData} {footer1} {footer2} />
</main>

<style>
  :root {
    background: rgb(244, 238, 233);
    overflow-y: hidden;
    color: rgb(28, 47, 62);
  }
</style>
