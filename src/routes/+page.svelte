<script>
	// @ts-nocheck
	import { browser } from '$app/environment';
	// import css
	import './page.css';
	import './font.css';
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
	function reload() {
		if (browser) {
			if ((h == 0 || h == 12) && min == 0 && s == 0) {
				window.location.reload();
			}
		}
	}
	setInterval(reload, 1000);

	// Setup countdown
	let urlstring;
	if (browser) {
		urlstring = window.location.href;
	}
	let now, targetHumane, target, interval;
	now = Date.now();
	setInterval(() => {
		now = Date.now();
	}, 1);
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
	target = new Date(
		Number(targetHumane.y),
		Number(targetHumane.mon) - 1,
		Number(targetHumane.d),
		Number(targetHumane.h),
		Number(targetHumane.min),
		Number(targetHumane.s)
	).getTime();
	let title = '高考';
	if (browser) {
		let url = new URL(urlstring);
		if (url.searchParams.get('title')) {
			title = url.searchParams.get('title');
		}
	}
	if (browser) {
		let url = new URL(urlstring);
		if (url.searchParams.get('timestamp')) {
			target = url.searchParams.get('timestamp');
		}
	}
	let id, week, day;
	$: {
		// id = Math.ceil(interval / 86400000);
		week =
			(new Date(
				new Date(target).getFullYear(),
				new Date(target).getMonth(),
				new Date(target).getDate()
			).getTime() -
				new Date(date.getFullYear(), date.getMonth(), date.getDate()).getTime() -
				(7 - date.getDay() + new Date(target).getDay()) * 86400000) /
			604800000;
		day = 7 - date.getDay() + new Date(target).getDay();
		id = interval = Math.ceil((target - now) / 86400000);
	}
</script>

<head>
	<title>Clock&Countdown</title>
	<!---
	<script
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
	--->
</head>

<div id="left" class="position-relative top-0 start-0">
	<div id="countdown" class="position-absolute top-0 start-0">
		<span style="font-size: large;">距离{title}还有&nbsp</span>{day}+{week}×7={id}<span
			style="font-size: large;"
			>天&nbsp
		</span>
	</div>
</div>

<div id="right" class="position-absolute top-50 start-50 translate-middle">
	<div id="time">
		<strong>{h}</strong><span class="colon">:</span><strong>{min}</strong><span class="colon"
			>:</span
		><strong style="color: red;">{s}</strong>
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
