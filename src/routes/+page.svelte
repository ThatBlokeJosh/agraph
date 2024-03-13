<script lang="ts">
	import {evaluate} from 'mathjs'
	import { page } from '$app/stores';
	import { scale } from 'svelte/transition';
	const minx = -50
	const maxx = 50

	let values: number[] = []
	let i = "/"
	let f: string = `sin($x)`
	console.log("You can change the icon with the ?icon query parameter or with typing i=")

	function draw() {
		values = []
		if (f.includes("i=") && f.length >= 3) {
			i = f.substr(2)
			values = []
			f = ""
			return
		}
		for (let x = minx; x <= maxx; x += 0.5 ) {
			try {
				values.push(evaluate(f.replaceAll('$x', `${x}`)))
			} catch {
				values = []
			}
		}
		values = values
	}

	function icon() {
		let p = $page.url.searchParams.get("icon")
		if (p) {
			return p
		} else {
			return i
		}
	}

	draw()

</script>

<style>
	.noselect {
	  -webkit-user-select: none; /* Safari */
	  -ms-user-select: none; /* IE 10 and IE 11 */
	  user-select: none; /* Standard syntax */
	}
	input {
		all: unset;
		position: absolute;
		bottom: 10vh;
		left: 0;
		right: 0;
		margin-left: auto;
		margin-right: auto;
		font-size: 50px;
		font-weight: 800;
		font-style: italic;
		color: white;
	}
</style>

<div class="relative w-screen h-screen grid justify-center items-center m-0 text-center bg-black">
	<input style="background-color: transparent;" type="text" bind:value={f} on:input={draw}>
	<div class="rounded-xl text-white m-auto overflow-hidden flex w-[100vw] h-[100vh] justify-center items-center mx-auto my-auto text-center noselect">
		{#each values as y}
			<div transition:scale|global="{{ duration: 200, delay: 100 }}"
			class={`text-7xl font-extrabold`}
			style:margin-bottom={`${(y)}%`}
			style:width={"1vw"}
			style:height={"1vh"}
			>{icon()}</div>
		{/each}
	</div>
</div>
