<script>
  // TODO: to implement hover effect on https://www.youtube.com/watch?v=htGfnF1zN4g

	import { onMount } from 'svelte';
  import { tilt } from "./effects.js"

  const tiltOptions = {
    scale: 1.1,
    // glare: true,
    // "max-glare": 0.8,
  }

  let cardHeight;
  let cardWidth;

  const _dispatchResize = () => {
    const ratio = 0.85;

    cardHeight = window.innerHeight * ratio;
    cardWidth = cardHeight * ratio / 3.5 * 2.5;
    if (cardWidth > window.innerWidth) {
      cardWidth = window.innerWidth * ratio;
      cardHeight = cardWidth / 2.5 * 3.5;
    }
  }

  let dispatchResize_timeout;
  const dispatchResize = () => {
    clearInterval(dispatchResize_timeout);
    dispatchResize_timeout = setTimeout(_dispatchResize, 400);
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
    <img class="card-image" src="https://placekitten.com/2000" alt="QR Code"/>
    <div class="card-descriptor">
      <div class="card-descriptor-left">
        <h1>Jin Lin</h1>
        <h2>Sincere Wizard</h2>
        <div class="event-details-bar">
          <span class="text">21 May 23</span>
          <div class="seperator"></div>
          <span class="text">1:30pm</span>
        </div>  
      </div>
      <div class="card-descriptor-right">
        <img class="ufinity-logo" alt="Ufinity Logo" src="https://media.glassdoor.com/sqll/385955/ufinity-singapore-squareLogo-1667211661299.png" />
      </div>  
    </div>   
  </div> 
  
</div>


<style>
  .card-container {
    width: calc( var(--cardWidth) );
    border: 1px solid lightgrey;
    border-radius: calc( var(--cardWidth) * 0.05);;
    background: rgb(255, 252, 232);
    display: flex;
  }

  .card-content {
    height: 100%; 
    margin: calc( var(--cardWidth) * 0.1 / 2);
    width: calc( var(--cardWidth) * 0.9 );
  }
  
  .card-image {
    width: 100%;
  }

  .card-descriptor {
    display: flex;
    flex-direction: row;
    height: calc( var(--cardHeight) - var(--cardWidth) );
    width: 100%;
  }

  .card-descriptor-left {
    display: flex;
    flex-direction: column;
    align-items: start;
    flex-grow: 1;
    max-width: calc( var(--cardWidth) * 0.61);
  }

  .card-descriptor-left h1 {
    font-size: calc( var(--cardWidth) * 0.12);
    margin-top: 0.5em;
    margin-bottom: 0;
    text-align: left;
    display: -webkit-box;
    -webkit-line-clamp: 2;
    -webkit-box-orient: vertical;  
    overflow: hidden;
  }

  .card-descriptor-left h2 {
    font-size: calc( var(--cardWidth) * 0.06);
    margin-top: 0;
    flex-grow: 1;
    text-align: left;
    width: 100%;
    text-overflow: ellipsis;
    overflow: hidden;
    white-space: nowrap;
  }

  .event-details-bar {
    display: flex;
    border: 1px solid black;
    border-radius: 0.25rem;
    font-size: calc( var(--cardWidth) * 0.04);
  }
  .event-details-bar .text {
    padding: 0.25em 0.5em;
  }
  .event-details-bar .seperator {
    width: 2em;
    border-left: 1px solid black;
    border-right: 1px solid black;
    background-image: linear-gradient(45deg, #808080 25%, transparent 25%), linear-gradient(-45deg, #808080 25%, transparent 25%), linear-gradient(45deg, transparent 75%, #808080 75%), linear-gradient(-45deg, transparent 75%, #808080 75%);
    background-size: 4px 4px;
    background-position: 0 0, 0 2px, 2px -2px, -2px 0px;
  }


  .card-descriptor-right {
    display: flex;
    align-items: end;
    justify-content: end;
    height:100%;
  }

  .ufinity-logo {
    position: relative;
    bottom: 0px;
    right: 0px;
    height: calc( var(--cardHeight) * 0.06);
    width: calc( var(--cardHeight) * 0.06 * 3);
    border-radius: 1em;
    object-fit: cover;
  }
</style>
