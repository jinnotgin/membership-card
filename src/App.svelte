<script>
	import Card from "./lib/Card.svelte";
	import { colord } from "colord";
	import { onMount } from "svelte";
	import LoadingState from "./lib/LoadingState.svelte"; // New component suggestion
	import ErrorState from "./lib/ErrorState.svelte"; // New component suggestion

	// API endpoint - make this configurable
	const API_BASE_URL = "http://localhost:8080/"; // Replace with your actual API endpoint

	// State variables
	let loading = false;
	let error = null;
	let userData = null;

	// Default values
	let name = "Jin";
	let slogan = "Corporate Strategy";
	let footer1 = "21 May 23";
	let footer2 = "1:30pm";
	let logo = "https://i.imgur.com/nfhw22v.png";
	let color = "4980c8";
	let data = { };

	// Parse URL parameters
	const urlParams = new URLSearchParams(window.location.search);
	const uuidParam = urlParams.get("uuid");

	// Override with URL params if provided
	if (urlParams.get("name")) name = urlParams.get("name");
	if (urlParams.get("slogan")) slogan = urlParams.get("slogan");
	if (urlParams.get("footer1")) footer1 = urlParams.get("footer1");
	if (urlParams.get("footer2")) footer2 = urlParams.get("footer2");
	if (urlParams.get("logo")) logo = urlParams.get("logo");
	if (urlParams.get("color")) color = urlParams.get("color");

	try {
		if (urlParams.get("data")) data = JSON.parse(urlParams.get("data"));
	} catch (e) {
		console.error(e);
	}

	// Fetch user data from API if UUID is provided
	async function fetchUserData(uuid) {
		loading = true;
		error = null;

		try {
			const response = await fetch(`${API_BASE_URL}?uuid=${uuid}`);

			if (!response.ok) {
				throw new Error(`Error: ${response.status} ${response.statusText}`);
			}

			const user = await response.json();

			// Update the card data with user information
			name = user.name || name;
			slogan = user.department || slogan;
			data = { ...data, uuid: user.uuid };

			userData = user;
		} catch (err) {
			console.error("Failed to fetch user data:", err);
			error = err.message || "Failed to load user data";
		} finally {
			loading = false;
		}
	}

	// Function to retry fetching user data
	function retryFetch() {
		if (uuidParam) {
			fetchUserData(uuidParam);
		}
	}

	onMount(() => {
		if (uuidParam) {
			fetchUserData(uuidParam);
		}
	});

	// Generate QR code data
	$: qrData = JSON.stringify({
		...data,
	});

	$: colorHex = `#${color.replace("#", "")}`;
	const generateMatchingColors = (colorStr) => {
		const MIN_BRIGHTNESS = 0.7;
		let bgColor = colord(colorStr);
		if (bgColor.brightness() < MIN_BRIGHTNESS / 2)
			bgColor = bgColor.lighten(0.4);
		else if (bgColor.brightness() < MIN_BRIGHTNESS)
			bgColor = bgColor.lighten(0.2);
		const background = bgColor.toHslString();

		let textColor = bgColor.invert();
		if (bgColor.brightness() > 0.5) textColor = textColor.darken(0.3);
		else textColor = textColor.lighten(0.3);
		const text = textColor.toHslString();

		const bgGradients = [
			bgColor.toHslString(),
			bgColor.rotate(10).toHslString(),
			bgColor.rotate(30).toHslString(),
		];

		return { background, text, bgGradients };
	};
	$: colors = generateMatchingColors(colorHex);
</script>

<main
	style="--color-text:{colors.text}; --color-bgGradient1:{colors
		.bgGradients[0]}; --color-bgGradient2:{colors
		.bgGradients[1]}; --color-bgGradient3:{colors.bgGradients[2]};"
>
	{#if loading}
		<LoadingState />
	{:else if error}
		<ErrorState message={error} retryFn={retryFetch} />
	{:else}
		<Card {name} {slogan} {qrData} {footer1} {footer2} {logo} />
	{/if}
</main>

<footer>
	Created by <a href="https://jinn.me" target="_blank" rel="noreferrer">Jin</a>
	| Source:
	<a
		href="https://github.com/jinnotgin/membership-card"
		target="_blank"
		rel="noreferrer">Github</a
	>
</footer>

<style>
	:root {
		background: hsl(27 20% 95%);
		color: hsl(206 38% 18%);
	}

	main {
		height: 100svh;
		display: flex;
		align-items: center;
		justify-content: center;
	}

	footer {
		padding-top: 8svh;
	}
	footer a {
		color: inherit;
		text-decoration: underline;
	}
</style>
