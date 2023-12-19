<script lang="ts">
	import { onMount } from 'svelte';
	// import Hydra from 'hydra-synth';

	let cv = undefined;
	let a = 1;
	let b = 0;
	let cur = 0;
	let prog = [
		`shape(a)
        	.add(osc(1, 0.5, 0.266), 1)
        	.add(o1, () => Math.sin(time / 4) * 0.846 + 0.119)
        	.scale(() => Math.sin(time / 16))
        	.rotate(0, -0.1)
        	.out(o1);
        // .repeat(b)
        src(o1)
        	.rotate(b, 0.1)
        	.out();`,
		`osc(a,-0.5, b).color(-1.5, -1.5, -1.5).blend(o0).rotate(-0.5, -0.5).modulate(shape(4).rotate(0.5, 0.5).scale(b).repeatX(2, 2).modulate(o0, () => mouse.x * 0.0005).repeatY(2, 2)).out(o0)
`,
		`osc(a).rotate(b, 0.1).modulate(osc()).out()`
		// From Hydra web
	];

	onMount(() => {
		const hydra = new Hydra({ detectAudio: false, canvas: cv });
		eval(prog[cur]);
	});
</script>

<div></div>

<div class="fixed bottom-0 m-8 w-[100vw] flex justify-center text-neutral-50">
	<div
		class="flex flex-col justify-center p-4 bg-neutral-800 border border-neutral-700 shadow-md rounded-md"
	>
		<div class="flex gap-2 justify-center items-center">
			<input
				type="range"
				name=""
				bind:value={a}
				on:change={() => eval(prog[cur])}
				step="1"
				min="1"
				max="10"
			/>
			{a}
			<input
				type="range"
				name=""
				bind:value={b}
				on:change={() => eval(prog[cur])}
				step="0.1"
				min="0"
				max="1"
			/>
			{b}

			<button
				class="border px-2 py-1 rounded-md ml-2"
				on:click={() => {
					cur = Math.floor(Math.random() * prog.length);
					eval(prog[cur]);
				}}>Shuffle</button
			>
		</div>
	</div>
</div>

<div class="h-[100vh] w-[100vw]">
	<canvas bind:this={cv} class="w-full h-full"></canvas>
</div>

<!-- <button on:click={() => osc(20).rotate(0, a).modulate(osc()).out()}> asdf </button> -->

<style>
	div {
		font-family: 'Zed Mono Extended';
	}
</style>
