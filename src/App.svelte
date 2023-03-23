<script>
  import Card from './lib/Card.svelte'
  import { colord } from "colord";

  const urlParams = new URLSearchParams(window.location.search);
  let name = urlParams.get('name') ? urlParams.get('name') : "Jin";
  let slogan = urlParams.get('slogan') ? urlParams.get('slogan') : "Consultant, Corporate Strategy";
  let footer1 = urlParams.get('footer1') ? urlParams.get('footer1') : "21 May 23";
  let footer2 = urlParams.get('footer2') ? urlParams.get('footer2') : "1:30pm";
  let logo = urlParams.get('logo') ? urlParams.get('logo') : "https://i.imgur.com/nfhw22v.png";
  let color = urlParams.get('color') ? urlParams.get('color') : '4980c8';

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
    const MIN_BRIGHTNESS = 0.70;
    let bgColor = colord(colorStr);
    if (bgColor.brightness() < MIN_BRIGHTNESS / 2) bgColor = bgColor.lighten(0.4);
    else if (bgColor.brightness() < MIN_BRIGHTNESS) bgColor = bgColor.lighten(0.2);
    const background = bgColor.toHslString();

    let textColor = bgColor.invert();
    if (bgColor.brightness() > 0.5) textColor = textColor.darken(0.3);
    else textColor = textColor.lighten(0.3);
    const text = textColor.toHslString();
    
    const bgGradients = [
      bgColor.toHslString(), 
      bgColor.rotate(10).toHslString(), 
      bgColor.rotate(30).toHslString()
    ];

    return {background, text, bgGradients};
  }
  $: colors = generateMatchingColors(colorHex);

</script>

<main 
style="--color-text:{colors.text}; --color-bgGradient1:{colors.bgGradients[0]}; --color-bgGradient2:{colors.bgGradients[1]}; --color-bgGradient3:{colors.bgGradients[2]};"
>
  <Card {name} {slogan} {qrData} {footer1} {footer2} {logo} />
</main>
<footer>
  Created by <a href="https://jinn.me" target="_blank" rel="noreferrer">Jin</a> | Source: <a href="https://github.com/jinnotgin/membership-card" target="_blank" rel="noreferrer">Github</a>
</footer>

<style>
  :root {
    background: hsl(27 20% 95%);
    color: hsl(206 38% 18%);
  }

  main {
    height: 100vh;
    display: flex;
    align-items: center;
  }
  
  footer {
    padding-top: 6em;
  }
  footer a {
    color: inherit;
    text-decoration: underline;
  }
</style>
