<script>
	import {crossfade, scale, fly} from 'svelte/transition';
    export let images;
    export let selected;
    export let load;
    export let updateCart;

    const [send, receive] = crossfade({
		duration: 200,
		fallback: scale
	});
</script>

<style>
	.cat-canvas {
		position: relative;
		display: flex;
		flex-direction: column;
		width: 80vmin;
		height: 80vmin;
	}
	.grid {
		display: grid;
		flex: 1;
		grid-template-columns: repeat(3, 1fr);
		grid-template-rows: repeat(4, 1fr);
		grid-gap: 2vmin;
	}
	.square button {	
		background-color: #001f3f;
		width: 100%;
		height: 100%;
		color: white;
		font-size: 2vmin;
		border: none;
		margin: 0;
		will-change: transform;
	}
	.photo, img {
		position: absolute;
		top: 0;
		left: 0;
		width: 100%;
		height: 100%;
		overflow: hidden;
	}
	.photo {
		display: flex;
		align-items: stretch;
		justify-content: flex-end;
		flex-direction: column;
		will-change: transform;
	}
	img {
		object-fit: cover;
		cursor: pointer;
	}
</style>

<div class="container">
	<div class="cat-canvas">
		<div class="grid">
			{#each images as image}
				<div class="square">
					{#if selected !== image}
						<button
							on:click="{() => {
								load(image)
								updateCart(image, 'push')}}"
							in:receive={{key:image.id}}
							out:send={{key:image.id}}>
							{image.name}
						</button>
					{/if}
				</div> 
			{/each}
		</div>
		{#if selected}
			{#await selected then d}
				<div class="photo" in:receive={{key:d.id}} out:send={{key:d.id}}>
					<img
						alt={d.alt}
						src={d.path}
						on:click="{() => {
							updateCart(d, 'pop')
							d = null }}"/>
				</div>
			{/await}
		{/if}
	</div>
</div>