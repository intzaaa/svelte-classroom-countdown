<script>
	// @ts-nocheck
	import { browser } from '$app/environment';
	// import css
	import './home.css';
	// Show time
	let date = new Date();
	setInterval(() => {
		date = new Date();
	}, 1);
	let y, mon, d, h, min, s;
	function addZero(num) {
		if (num < 10) {
			num = String(0) + String(num);
		}
		return num;
	}
	$: {
		y = date.getFullYear();
		mon = addZero(date.getMonth() + 1);
		d = addZero(date.getDate());
		h = addZero(date.getHours());
		min = addZero(date.getMinutes());
		s = addZero(date.getSeconds());
	}

	// Reload every night
	// function reload() {
	// 	onMount(() => window.location.reload());
	// }
	// setInterval(reload, 1000);

	// Setup countdown
	let urlstring;
	if (browser) {
		urlstring = window.location.href;
	}
	let now, targetHumane, target, interval;
	now = Date.now();
	targetHumane = {
		y: date.getFullYear() + 1,
		mon: 6,
		d: 7,
		h: 0,
		min: 0,
		s: 0
	};
	if (browser) {
		let url = new URL(urlstring);
		if (url.searchParams.get('year')) {
			targetHumane.y = url.searchParams.get('year');
		}
		if (url.searchParams.get('month')) {
			targetHumane.mon = url.searchParams.get('month');
		}
		if (url.searchParams.get('day')) {
			targetHumane.d = url.searchParams.get('day');
		}
		if (url.searchParams.get('hour')) {
			targetHumane.h = url.searchParams.get('hour');
		}
		if (url.searchParams.get('minute')) {
			targetHumane.min = url.searchParams.get('minute');
		}
		if (url.searchParams.get('second')) {
			targetHumane.s = url.searchParams.get('second');
		}
	}
	target = Date.parse(
		targetHumane.y +
			'/' +
			targetHumane.mon +
			'/' +
			targetHumane.d +
			' ' +
			targetHumane.h +
			':' +
			targetHumane.min +
			':' +
			targetHumane.s
	);
	if (browser) {
		let url = new URL(urlstring);
		if (url.searchParams.get('timestamp')) {
			target = url.searchParams.get('timestamp');
		}
	}
	interval = target - now;
	let id;
	$: {
		id = Math.ceil(interval / 86400000);
	}
</script>

<head
	><script
		src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0-alpha1/dist/js/bootstrap.bundle.min.js"
		integrity="sha384-w76AqPfDkMBDXo30jS1Sgez6pr3x5MlQ1ZAGC+nuZB+EYdgRZgiwxhTBTkF7CXvN"
		crossorigin="anonymous"
	></script>
	<link
		href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0-alpha1/dist/css/bootstrap.min.css"
		rel="stylesheet"
		integrity="sha384-GLhlTQ8iRABdZLl6O3oVMWSktQOp6b7In1Zl3/Jr59b6EGGoI1aFkw7cmDA6j6gD"
		crossorigin="anonymous"
	/>
</head>

<div id="left" class="position-relative top-0 start-0">
	<div id="countdown" class="position-absolute top-0 start-0">
		{id}<span style="font-size: large;">天&nbsp&nbsp</span>
	</div>
</div>

<div id="right" class="position-absolute top-50 start-50 translate-middle">
	<div id="time">
		<b>{h}</b><span class="colon">:</span><b>{min}</b><span class="colon">:</span><b>{s}</b>
	</div>
	<div id="date">-{y}<span class="dot">.</span>{mon}<span class="dot">.</span>{d}-</div>
</div>

<style>
	:global(body) {
		background-image: url(https://bing.biturl.top/?resolution=3840&format=image&index=0&mkt=zh-CN) !important;
		background-size: cover !important;
		background-repeat: no-repeat !important;
	}
</style>
