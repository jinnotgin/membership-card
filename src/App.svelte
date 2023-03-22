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
  <footer>
    <a href="https://github.com/jinnotgin/membership-card" target="_blank" rel="noreferrer" title="Jin, 2023">Github Source</a>
  </footer>
</main>

<style>
  :root {
    background: rgb(244, 238, 233);
    overflow-y: hidden;
    color: rgb(28, 47, 62);
  }
  footer {
    position: absolute;
    bottom: 0;
    left: 0;
    padding: 0.25em 0.5em;
    background: white;
  }
  footer a {
    color: black;
  }
</style>
