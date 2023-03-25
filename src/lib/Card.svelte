<script>
  export let name, slogan, qrData, footer1, footer2, logo;

  import { tilt } from "./effects.js";
  import QrCode from "svelte-qrcode";

  // https://stackoverflow.com/a/36673184
  const isTouchDevice = (navigator.maxTouchPoints || 'ontouchstart' in document.documentElement);

  const tiltOptions = {
    scale: isTouchDevice ? 1.0 : 1.1,
  }

  // user feedback was that motion prompt was scary. hence, decided to forgo iOS devices all together
  // https://github.com/micku7zu/vanilla-tilt.js/issues/71#issuecomment-769294652
  /*const deviceMotionPermissionRequestor = () => {
  try {
        //try block bc Safari
        if (typeof DeviceMotionEvent.requestPermission === "function") {
          DeviceMotionEvent.requestPermission()
            .then((permissionState) => {
              if (permissionState === "granted") {
                window.addEventListener("devicemotion", (e) => {
                  console.log(e);
                });
              }
            })
            .catch(console.error);
        } else {
          // handle regular non iOS 13+ devices
        }
      } catch (e) {}
  }*/

  // hover effect from https://www.youtube.com/watch?v=htGfnF1zN4g
  let isMouseIn = false;
  $: hoverOpacity = (isTouchDevice || !isMouseIn) ? 0 : 1;
  const mouse = { x: 0, y: 0 }
  const handleMousemove = event => {
    if (isTouchDevice) return false;
    isMouseIn = true;

    const { currentTarget: target } = event;
    const rect = target.getBoundingClientRect();

		mouse.x = event.clientX - rect.left;
		mouse.y = event.clientY - rect.top;
	}
  const handleMouseout = event => {
    if (isTouchDevice) return false;
    isMouseIn = false;
	}

  // rotating gradient border effect - degree changer. (not using, currently relying on experimental CSS)
  // rotating colours effect won't be shown on iOS as a result.
  // let gradientAngle = 0;
  // let gradientAngle_interval;
  // gradientAngle_interval = setInterval(() => {
  //   gradientAngle = gradientAngle >= 359 ? 0 : gradientAngle + 1;
  // }, 50);
</script>

<!-- <svelte:window on:click={deviceMotionPermissionRequestor} /> -->
<div 
  class="card-container" 
  use:tilt={tiltOptions}
  style="--gradient-angle:180deg;"
> <!-- defining gradient-angle here is for browsers that dont support @property -->
  <div class="card-content"
  on:mousemove={handleMousemove} 
  on:mouseout={handleMouseout} 
  on:blur={handleMouseout} 
  style="--hoverOpacity:{hoverOpacity}; --mouseX:{mouse.x}px; --mouseY:{mouse.y}px"
  >
    <div class="card-image">
      <QrCode size={1280} padding={36} value={qrData} errorCorrection="H" />
    </div>
    <!-- <img class="card-image" src="https://placekitten.com/2000" alt="QR Code"/> -->
    <div class="card-descriptor">
      <h1>{name}</h1>
      <h2>{slogan}</h2>
      <div class="card-footer">
        <div class="card-footer-left">
          <div class="event-details-bar">
            <span class="text">{footer1}</span>
            <div class="seperator"></div>
            <span class="text">{footer2}</span>
          </div>  
        </div>
        <div class="card-footer-right">
          <img class="logo-image" alt="logo" src={logo} />
        </div>  
      </div>
    </div>   
  </div> 
  
</div>


<style>
  :global(img.qrcode) {
    width: 100%;
  }

  /* rotating circular gradient: https://www.youtube.com/watch?v=-VOUK-xFAyk */
  @property --gradient-angle {
    syntax: "<angle>";
    initial-value: 0deg;
    inherits: false;
  }
  @keyframes rotation {
    0%  { --gradient-angle: 180deg; }
    100%  { --gradient-angle: 540deg; }
    /* this is set this way, so that on devices that don't support @property, the color looks better */
  }
  .card-container {
    height: 80svmin;
    aspect-ratio: 1 / 1.5;
    border: 1px solid lightgrey;
    border-radius: 2svmin;
    display: flex;
    user-select: none;
    position: relative;
  }

  @media only screen and (max-width: 576px) {
    .card-container {
      height: 100svmin;
    }
  }

  .card-container::before {
    content: "";
    position: absolute;
    inset: 0;
    z-index: -1;
    filter: blur( 5svmin );
  }
  .card-container, 
  .card-container::before {
    background: conic-gradient(
      from var(--gradient-angle),
      var(--color-bgGradient1),
      var(--color-bgGradient2),
      var(--color-bgGradient3),
      var(--color-bgGradient2),
      var(--color-bgGradient1)
    );
    animation: rotation 15s linear infinite;
  }

  .card-content {
    padding: 3.75svmin 4svmin;
    width: 100%;
    color: var(--color-text);
    border-radius: inherit;
  }
  .card-content::before {
    content: "";
    opacity: var(--hoverOpacity);
    background: radial-gradient(
      75svmin circle at var(--mouseX) var(--mouseY), 
      rgba(255, 255, 255, 0.3), 
      transparent 40%
    );
    border-radius: inherit;
    position: absolute;
    inset: 0;
    height: 100%;
    width: 100%;
    z-index: 10;
  }

  .card-image {
    height: 63%;
    box-sizing: border-box;
    background: white;
    border: 0.65svmin dashed var(--color-text);
    border-radius: 1.5svmin;
  }
  
  .card-descriptor {
    height: 37%;
    display: flex;
    flex-direction: column;
    width: 100%;
    align-items: start;
    text-align: left;
  }

  .card-descriptor h1 {
    font-size: 4.75svmin;
    margin-top: 0.25em;
    margin-bottom: 0;
  }
  .card-descriptor h2 {
    font-size: 2.75svmin;
    font-family: monospace;
    font-weight: 200;
    margin-top: 0;
    flex-grow: 1;
  }

  .card-descriptor h1,
  .card-descriptor h2 {
    display: -webkit-box;
    -webkit-line-clamp: 2;
    -webkit-box-orient: vertical;  
    overflow: hidden;
  }

  .card-footer {
    height: 15%;
    width: 100%;
    display: flex;
    align-items: end;
  }

  .card-footer-left {
    display: flex;
    width: 70%;
  }

  .event-details-bar {
    display: flex;
    border: 0.1em solid var(--color-text);
    border-radius: 0.25em;
    font-size: 2.1svmin;
    font-weight: 600;
  }
  .event-details-bar .text {
    padding: 0.15em 0.5em;
  }
  .event-details-bar .seperator {
    width: 1.5em;
    border-left: 0.1em solid var(--color-text);
    border-right: 0.1em solid var(--color-text);
    background-image: linear-gradient(45deg, #808080 25%, transparent 25%), linear-gradient(-45deg, #808080 25%, transparent 25%), linear-gradient(45deg, transparent 75%, #808080 75%), linear-gradient(-45deg, transparent 75%, #808080 75%);
    background-size: 4px 4px;
    background-position: 0 0, 0 2px, 2px -2px, -2px 0px;
  }

  .card-footer-right {
    display: flex;
    align-items: end;
    justify-content: end;
    width: 30%;
  }

  .logo-image {
    width: 100%;
    aspect-ratio: 3.25 / 1;
    border-radius: 1.2svmin;
    object-fit: cover;
  }
</style>
