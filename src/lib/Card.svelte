<script>
  // TODO: to implement hover effect on https://www.youtube.com/watch?v=htGfnF1zN4g
  export let name, slogan, qrData, eventDate, eventTime;

	import { onMount } from 'svelte';
  import { tilt } from "./effects.js";
  import QrCode from "svelte-qrcode";

  const isMobile = window.matchMedia('only screen and ((max-width: 767px) or (max-height: 767px))').matches;
  const tiltOptions = {
    scale: isMobile ? 1.0 : 1.1,
    // glare: true,
    // "max-glare": 0.8,
  }

  let cardHeight;
  let cardWidth;

  const _dispatchResize = () => {
    const viewport = window.visualViewport;
    const viewportPercent_height = 0.80;
    const viewportPercent_width = 0.75;
    const cardHeightRatio = 1.5;

    cardWidth = viewport.width * viewportPercent_width;
    cardHeight = cardWidth * cardHeightRatio;
    if (cardHeight > viewport.height * viewportPercent_height) {
      cardHeight = viewport.height * viewportPercent_height;
      cardWidth = cardHeight / cardHeightRatio;
    }
  }
  _dispatchResize();

  let dispatchResize_debounce;
  const dispatchResize = () => {
    clearInterval(dispatchResize_debounce);
    dispatchResize_debounce = setTimeout(_dispatchResize, 500);
  }

  // https://github.com/micku7zu/vanilla-tilt.js/issues/71#issuecomment-769294652
  const deviceMotionPermissionRequestor = () => {
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
  }

	onMount(async () => {
    dispatchResize();
	});
</script>

<svelte:window on:resize={dispatchResize} on:click={deviceMotionPermissionRequestor} />

<div class="card-container" use:tilt={tiltOptions} style="--cardHeight:{cardHeight}px; --cardWidth:{cardWidth}px" >
  <div class="card-content">
    <div>
      <QrCode size={cardWidth} padding={cardWidth*0.02} value={qrData} errorCorrection="H" />
    </div>
    <!-- <img class="card-image" src="https://placekitten.com/2000" alt="QR Code"/> -->
    <div class="card-descriptor">
      <h1>{name}</h1>
      <h2>{slogan}</h2>
      <div class="card-footer">
        <div class="card-footer-left">
          <div class="event-details-bar">
            <span class="text">{eventDate}</span>
            <div class="seperator"></div>
            <span class="text">{eventTime}</span>
          </div>  
        </div>
        <div class="card-footer-right">
          <img class="ufinity-logo" alt="Ufinity Logo" src="https://media.glassdoor.com/sqll/385955/ufinity-singapore-squareLogo-1667211661299.png" />
        </div>  
      </div>
    </div>   
  </div> 
  
</div>


<style>
  :global(img.qrcode) {
    width: 100%;
    border: calc( var(--cardWidth) * 0.01) dashed rgb(25, 42, 54);
    border-radius: calc( var(--cardWidth) * 0.015);
  }

  .card-container {
    width: calc( var(--cardWidth) );
    height: calc( var(--cardHeight) );
    border: 1px solid lightgrey;
    border-radius: calc( var(--cardWidth) * 0.05);
    background: rgb(255, 252, 232);
    display: flex;
    user-select: none;
  }

  .card-content {
    height: 100%; 
    margin: calc( var(--cardWidth) * 0.15 / 2.5) calc( var(--cardWidth) * 0.15 / 2);
    width: calc( var(--cardWidth) * 0.85 );
  }
  
  .card-descriptor {
    display: flex;
    flex-direction: column;
    height: calc( var(--cardHeight) - var(--cardWidth) );
    width: 100%;
    align-items: start;
  }

  .card-descriptor h1 {
    font-size: calc( var(--cardWidth) * 0.09);
    margin-top: 0.25em;
    margin-bottom: 0;
    text-align: left;
    display: -webkit-box;
    -webkit-line-clamp: 2;
    -webkit-box-orient: vertical;  
    overflow: hidden;
  }

  .card-descriptor h2 {
    font-family: monospace;
    font-weight: 200;
    font-size: calc( var(--cardWidth) * 0.05);
    margin-top: 0;
    flex-grow: 1;
    text-align: left;
    width: 100%;
    display: -webkit-box;
    -webkit-line-clamp: 2;
    -webkit-box-orient: vertical;  
    overflow: hidden;
  }

  .card-footer {
    width: 100%;
    display: flex;
    align-items: end;
  }

  .card-footer-left {
    display: flex;
    flex-direction: column;
    align-items: start;
    flex-grow: 1;
  }

  .event-details-bar {
    display: flex;
    border: 2px solid black;
    border-radius: 0.25rem;
    font-size: calc( var(--cardWidth) * 0.04);
    font-weight: 600;
  }
  .event-details-bar .text {
    padding: 0.15em 0.5em;
  }
  .event-details-bar .seperator {
    width: 1.5em;
    border-left: 2px solid black;
    border-right: 2px solid black;
    background-image: linear-gradient(45deg, #808080 25%, transparent 25%), linear-gradient(-45deg, #808080 25%, transparent 25%), linear-gradient(45deg, transparent 75%, #808080 75%), linear-gradient(-45deg, transparent 75%, #808080 75%);
    background-size: 4px 4px;
    background-position: 0 0, 0 2px, 2px -2px, -2px 0px;
  }

  .card-footer-right {
    display: flex;
    align-items: end;
    justify-content: end;
    height:100%;
  }

  .ufinity-logo {
    height: calc( var(--cardHeight) * 0.06);
    width: calc( var(--cardHeight) * 0.06 * 3);
    border-radius: 1em;
    object-fit: cover;
  }
</style>
