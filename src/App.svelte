<script>
  import Card from './lib/Card.svelte'

  import { colord, extend } from "colord";
  import harmoniesPlugin from "colord/plugins/harmonies";
  extend([harmoniesPlugin]);

  const urlParams = new URLSearchParams(window.location.search);
  let name = urlParams.get('name') ? urlParams.get('name') : "Jin";
  let slogan = urlParams.get('slogan') ? urlParams.get('slogan') : "Consultant, Corporate Strategy";
  let footer1 = urlParams.get('footer1') ? urlParams.get('footer1') : "21 May 23";
  let footer2 = urlParams.get('footer2') ? urlParams.get('footer2') : "1:30pm";
  let logo = urlParams.get('logo') ? urlParams.get('logo') : "https://i.imgur.com/nfhw22v.png";
  let color = urlParams.get('color') ? urlParams.get('color') : 'fffce8';

  let data = {};
  try {
    data = urlParams.get('data') ? JSON.parse(urlParams.get('data')) : {"id": "AF000001"};
  } catch (e) {
    console.error(e);
  }
  let qrData = JSON.stringify({
    name, ...data
  });

  $: colorHex = `#${color.replace("#","")}`;
  const generateMatchingColors = (colorStr) => {
    const inputColor = colord(colorStr);

    const MIN_BRIGHTNESS = 0.7;
    let bgColor = inputColor;
    if (inputColor.brightness() < MIN_BRIGHTNESS) bgColor = inputColor.lighten(MIN_BRIGHTNESS - inputColor.brightness());
    const background = bgColor.toHslString();

    const text = bgColor.invert().darken(0.2).toHslString();
    
    let darkColor = inputColor;
    if (inputColor.isLight()) darkColor = inputColor.darken(inputColor.brightness() - 0.5);
    const harmonies = darkColor.harmonies("complementary").map((c) => c.toHslString());

    return {background, text, harmonies};
  }
  $: colors = generateMatchingColors(colorHex);

</script>

<main 
style="--color-background:{colors.background}; --color-text:{colors.text}; --colors-harmony1:{colors.harmonies[0]}; --colors-harmony2:{colors.harmonies[1]};"
>
  <Card {name} {slogan} {qrData} {footer1} {footer2} {logo} />
</main>
<footer>
  Created by <a href="https://jinn.me" target="_blank" rel="noreferrer">Jin</a> | Source: <a href="https://github.com/jinnotgin/membership-card" target="_blank" rel="noreferrer">Github</a>
</footer>

<style>
  :root {
    background: rgb(244, 238, 233);
    color: rgb(28, 47, 62);
  }

  main {
    height: 100vh;
    display: flex;
    align-items: center;
  }
  
  footer {
    padding-top: 2em;
  }
  footer a {
    color: inherit;
    text-decoration: underline;
  }
</style>
