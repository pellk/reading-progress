<script>
import { onMount } from 'svelte';
const DAY_IN_MS = 24 * 60 * 60 * 1000;
let title = 'Jane Eyre';
let start = 154;
let end = 8704;
let startDate = '2023-02-23';
let days = 100;
let current = 3613;
$: progress = ((current - start) / (end - start)) * 100;
$: now =
	((new Date().getTime() - new Date(startDate).getTime()) /
		(new Date(startDate).getTime() + days * DAY_IN_MS - new Date(startDate).getTime())) *
	100;

onMount(() => {
	const params = localStorage.getItem('params');
	if (params) ({ title, start, end, current, startDate, days } = JSON.parse(params));
});

function save() {
	localStorage.setItem('params', JSON.stringify({ title, start, end, current, startDate, days }));
}
</script>

<svelte:head>
	<title>Reading Progress</title>
</svelte:head>

<main>
	<input class="title" bind:value={title} />
	<section class="fields">
		<fieldset>
			<label for="start">Start:</label>
			<input id="start" type="number" bind:value={start} />
		</fieldset>
		<fieldset>
			<label for="current">Current:</label>
			<input id="current" type="number" bind:value={current} />
		</fieldset>
		<fieldset>
			<label for="end">End:</label>
			<input id="end" type="number" bind:value={end} />
		</fieldset>
		<fieldset>
			<label for="startDate">Date:</label>
			<input id="startDate" type="date" bind:value={startDate} />
		</fieldset>
		<fieldset>
			<label for="days">Days:</label>
			<input id="days" type="number" bind:value={days} />
		</fieldset>
		<button type="button" on:click={save}>Save</button>
	</section>
	<div class="chart">
		<div class="progress" style:width="{progress}%">
			{Math.round(progress)}%
		</div>
		<div class="now" style:width="{now}%" />
		<div class="due">Due: {Math.round((now / 100) * (end - start) + start)}</div>
	</div>
</main>

<style>
* {
	font-family: Roboto, Segue UI, sans-serif;
	font-size: 14pt;
	color: #444;
}
main {
	display: flex;
	flex-direction: column;
	gap: 1em;
}
.title {
	font-family: Georgia, serif;
	font-size: 18pt;
	font-weight: bold;
	border: 1px solid #ccc;
}
section.fields {
	display: grid;
	grid-template-columns: auto auto auto;
	justify-content: start;
	align-items: end;
	gap: 0.5em;
	justify-items: center;
}
fieldset {
	border: none;
	padding: 0;
	display: grid;
	grid-template-columns: 70px 140px;
	gap: 0.5em;
	align-items: center;
}
fieldset label {
	text-align: right;
}
fieldset input {
	width: 100%;
}
.chart {
	background: #ccc;
	display: grid;
}
.chart > * {
	font-weight: bold;
	color: #fff;
	font-size: 18pt;
	display: grid;
	place-items: center;
	height: 2em;
	grid-column: 1 / 2;
	grid-row: 1 / 2;
}
.progress {
	background: #2255ff88;
	z-index: 1;
}
.now {
	background: #ff220088;
}
.due {
	justify-self: right;
	padding: 0 0.5em;
	z-index: 2;
	opacity: 0.8;
}
</style>
