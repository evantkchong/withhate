<script>
	import { onMount } from 'svelte';
	import { browser } from '$app/environment';

	// Inspired by https://dev.to/bryce/dvd-corner-bounces-but-more-satisfying-1355

	let left = 0;
	let top = 0;
	let logoWidth = 256;
	let logoHeight = 130;
	let formWidth = 700;
	let formHeight = 900;
	let color = {
		r: 255,
		g: 0,
		b: 0
	};

	// These control the movement of the logo
	$: leftPx = `${left}px`;
	$: topPx = `${top}px`;
	$: dvdFill = `rgb(${color.r},${color.g},${color.b})`;

	let looper;
	let speed = 6;
	let direction = {
		x: 1,
		y: 1
	};

	onMount(async () => {
		if (window.innerWidth <= 640) {
			logoWidth = 128;
			logoHeight = 65;
		}
		startMoving();
	});

	const updateWindowSize = () => {
		formWidth = window.innerWidth < 700 ? window.innerWidth - 5 : 700;
		formHeight = window.innerHeight < 1000 ? Math.floor(window.innerHeight * 0.8) : 1000;
	};
	if (browser) {
		updateWindowSize(); // to set the initial window size
		window.onresize = updateWindowSize; // run when ever the window size change
	}

	const randomBetween = (min, max) => min + Math.floor(Math.random() * (max - min + 1));

	function newColor() {
		color.r = randomBetween(0, 255);
		color.g = randomBetween(0, 255);
		color.b = randomBetween(0, 255);
	}

	function newSpeed() {
		speed = randomBetween(2, 6);
	}

	function move() {
		let { x, y } = direction;

		let newLeft = left + x * speed;
		const hitX = newLeft > window.innerWidth - logoWidth || newLeft < 0;

		let newTop = top + y * speed;
		const hitY = newTop > window.innerHeight - logoHeight || newTop < 0;

		// bounce
		if (hitX && hitY) {
			newColor();
			x *= -1;
			y *= -1;

			newLeft = left + x * speed;
			newTop = top + y * speed;
			newSpeed();
		} else if (hitX) {
			newColor();
			x *= -1;
			newLeft = left + x * speed;
			newSpeed();
		} else if (hitY) {
			newColor();
			y *= -1;
			newTop = top + y * speed;
			newSpeed();
		}

		if (hitX || hitY) {
			direction = { x, y };
		}

		left = newLeft;
		top = newTop;
	}

	function startMoving() {
		looper = window.requestAnimationFrame(startMoving);
		move();
	}
</script>

<div id="background" style="height:0">
	<div id="DVD" style="left: {leftPx}; top: {topPx}">
		<a href="https://www.youtube.com/watch?v=dQw4w9WgXcQ">
			<svg
				xmlns="http://www.w3.org/2000/svg"
				width="100%"
				height="100%"
				fill={dvdFill}
				viewBox="0 0 210 107"
				><title>DVD logo</title><path
					d="M118.895,20.346c0,0-13.743,16.922-13.04,18.001c0.975-1.079-4.934-18.186-4.934-18.186s-1.233-3.597-5.102-15.387H81.81H47.812H22.175l-2.56,11.068h19.299h4.579c12.415,0,19.995,5.132,17.878,14.225c-2.287,9.901-13.123,14.128-24.665,14.128H32.39l5.552-24.208H18.647l-8.192,35.368h27.398c20.612,0,40.166-11.067,43.692-25.288c0.617-2.614,0.53-9.185-1.054-13.053c0-0.093-0.091-0.271-0.178-0.537c-0.087-0.093-0.178-0.722,0.178-0.814c0.172-0.092,0.525,0.271,0.525,0.358c0,0,0.179,0.456,0.351,0.813l17.44,50.315l44.404-51.216l18.761-0.092h4.579c12.424,0,20.09,5.132,17.969,14.225c-2.29,9.901-13.205,14.128-24.75,14.128h-4.405L161,19.987h-19.287l-8.198,35.368h27.398c20.611,0,40.343-11.067,43.604-25.288c3.347-14.225-11.101-25.293-31.89-25.293h-18.143h-22.727C120.923,17.823,118.895,20.346,118.895,20.346L118.895,20.346z"
				/><path
					d="M99.424,67.329C47.281,67.329,5,73.449,5,81.012c0,7.558,42.281,13.678,94.424,13.678c52.239,0,94.524-6.12,94.524-13.678C193.949,73.449,151.664,67.329,99.424,67.329z M96.078,85.873c-11.98,0-21.58-2.072-21.58-4.595c0-2.523,9.599-4.59,21.58-4.59c11.888,0,21.498,2.066,21.498,4.59C117.576,83.801,107.966,85.873,96.078,85.873z"
				/><polygon
					points="182.843,94.635 182.843,93.653 177.098,93.653 176.859,94.635 179.251,94.635 178.286,102.226 179.49,102.226 180.445,94.635 182.843,94.635"
				/><polygon
					points="191.453,102.226 191.453,93.653 190.504,93.653 187.384,99.534 185.968,93.653 185.013,93.653 182.36,102.226 183.337,102.226 185.475,95.617 186.917,102.226 190.276,95.617 190.504,102.226 191.453,102.226"
				/></svg
			>
		</a>
	</div>
</div>

<div id="parent" style="height:100%; display:flex; flex-direction: column; align-items: center;">
	<div style="display:flex; flex-direction: column; width: fit-content;">
		<div style="display:flex; align-items: center; justify-content: center;">
			<div style="width: 90%">
				<p class="rainbow rainbow_text_animated">Hello please RSVP</p>
			</div>
		</div>
		<div>
			<div>
				<iframe
					title="Google Form"
					src="https://docs.google.com/forms/d/e/1FAIpQLScRRVBIzvkejrjBIJFx2PR7q7n2F5vq1x32BHrduWdWws0Isw/viewform?embedded=true"
					width={formWidth}
					height={formHeight}
					scrolling="yes"
					frameborder="1"
					marginheight="0"
					marginwidth="0"
					style="overflow-y: scroll;"
					class="rainbow rainbow_border_animated">Loading…</iframe
				>
			</div>
			<div style="display:flex; justify-content: center;">
				<div style="width: 90%; display:flex; justify-content: center;">
					<img
						src="https://images.neopets.com/template_images/zafara_blue_bounce.gif"
						alt="a bouncing zafara"
						class="zafara_img"
					/>
					<img
						src="https://images.neopets.com/template_images/zafara_blue_bounce.gif"
						alt="a bouncing zafara"
						class="zafara_img"
					/>
					<img
						src="https://images.neopets.com/template_images/zafara_blue_bounce.gif"
						alt="a bouncing zafara"
						class="zafara_img"
					/>
				</div>
			</div>
		</div>
	</div>
</div>

<style>
	:global(body) {
		font-family: 'Comic Sans', 'Comic Sans MS', 'Chalkboard', 'ChalkboardSE-Regular', sans-serif;
		color: yellow;
		background-image: url('/ssc2019-15b-med.webp');
	}

	.rainbow_text_animated {
		background: linear-gradient(to right, #b2a4ff, #bbe9ff, #bae5e5, #ffb4b4, #b2a4ff);
		-webkit-background-clip: text;
		background-clip: text;
		color: transparent;
		animation: rainbow_animation 2s ease-in-out infinite;
		background-size: 400% 100%;
	}

	.rainbow_border_animated {
		background: linear-gradient(to right, #b2a4ff, #bbe9ff, #bae5e5, #ffb4b4, #b2a4ff);
		-webkit-background-clip: border-box;
		background-clip: border-box;
		color: transparent;
		animation: rainbow_animation 2s ease-in-out infinite;
		background-size: 400% 100%;
	}

	.zafara_img {
		width: 10vw;
		max-width: 100px;
		height: 10vw;
		max-height: 100px;
		padding: 5px;
	}

	@keyframes rainbow_animation {
		0%,
		100% {
			background-position: 0 0;
		}

		50% {
			background-position: 100% 0;
		}
	}

	p {
		font-size: 18px;
	}

	@font-face {
		font-family: 'Ramilas';
		src: url('./tt-ramilas-trial.ttf');
	}

	#DVD {
		width: 128px;
		height: 65px;
		position: fixed;
	}

	@media (min-width: 640px) {
		#DVD {
			width: 256px;
			height: 130px;
			position: fixed;
		}
	}
</style>
