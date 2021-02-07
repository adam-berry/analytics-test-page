<script>
	import {crossfade, scale, fly} from 'svelte/transition';

	const [send, receive] = crossfade({
		duration: 200,
		fallback: scale
	});
	const randomOrder = toString(Math.floor(10000 * Math.random()));
	$: cart = {
					order_reference : randomOrder,
					total_amount: 100,
					currency: "USD",
					items: [],
					discounts: [],
					metadata: {}
				}
	let hints = {};
	let callbacks = {};

	$: {
		if (typeof BoltCheckout !== 'undefined') {
			BoltCheckout.configure(cart, hints, callbacks, {floatingButtonMode: 'show_cart'})
		}

	}
	function updateCart(item, method){
		if (method === 'pop'){
			cart.items.pop()
			console.log(cart)
			if (typeof BoltCheckout !== 'undefined') {
				BoltCheckout.configure(cart, hints, callbacks, {floatingButtonMode: 'show_cart'})
			}
		} else {
			let tmp = {
					name: item.name,
					reference: item.id,
					quantity: 1,
					image_url: item.path
			}
			cart.items.push(tmp)
			if (typeof BoltCheckout !== 'undefined') {
				BoltCheckout.configure(cart, hints, callbacks, {floatingButtonMode: 'show_cart'})
			}
		}	
	}




	const images = [
		{
			id: '1',
			alt: 'Crepuscular rays',
			path: '/bandana-cat.jpeg',
			name: 'Henry',
			price: 1000

		},
		{
			id: '2',
			alt: 'Lapland winter scene',
			path:'/black-cat.jpeg',
			name: 'Annette',
			price: 1000

		},
		{
			id: '3',
			alt: 'Jellyfish',
			path:'/flower-cat.jpeg',
			name: 'Lucy',
			price: 1000

		},
		{
			id: '4',
			alt: 'A man scuba diving',
			path:'/king-cat.jpeg',
			name: 'Randolph',
			price: 1000
		},
		{
			id: '5',
			alt: 'Scary looking cat named Tammy',
			path:'/pexels.jpeg',
			name: 'Tammy',
			price: 1000
		},
		{
			id: '6',
			alt: 'A white cat also named Tammy',
			path:'/white-cat.jpeg',
			name: 'Tammy II',
			price: 1000

		},
		{
			id: '7',
			alt: 'Two cats playing',
			path:'/cellino-and-barnes.jpeg',
			name:'Cellino and Barnes'  ,
			price: 1000
			

		},
		{
			id: '8',
			alt: 'A cat with a mustache',
			path:'/mustache-cat.jpeg',
			name: 'Howard',
			price: 1000
		},
		{
			id: '9',
			alt: 'Cat with its paws crossed looking skeptical',
			path:'/mad-cat.jpeg',
			name: 'Doris',
			price: 1000
		},
		{
			id: '10',
			alt: 'White cat looking hopeful',
			path:'/van-cat.jpeg',
			name: 'Terry',
			price: 1000
		},
		{
			id: '11',
			alt: 'Surprised cat on a couch',
			path:'/wtf-cat.jpeg',
			name: 'Tim',
			price: 1000
		},
		{
			id: '12',
			alt: 'The Stereophonics',
			path:'/cute-cat.jpeg',
			name: 'Mable',
			price: 1000
		}
	];
		
		
	$: selected = null;
	let loading = null;

	const load = image => {
			selected = image;
			loading = null;
		}
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
	.side-cart-container{
		position: relative;
	}
	.side-cart {
		position:absolute;
		width: 250px;
		font-size: larger;
		transform: translateY(-100px);
	}

	.side-cart-inner {
		text-align: center;
	}



</style>

<svelte:head>
	<title>Sapper project template</title>
</svelte:head>

<div class="container">
	<div class="cat-canvas">
		<!-- <h1>Gallery</h1> -->
		<div class="grid">
			{#each images as image}
				<div class="square">
					{#if selected !== image}
						<button
							on:click="{() => {
								load(image)
								updateCart(image, 'push')

							}}"
							in:receive={{key:image.id}}
							out:send={{key:image.id}}
							>{loading === image ? '...': image.name}
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
								d = null
								}	
							}"
						>
					</div>
			{/await}
		{/if}
	</div>

</div>

	<div class='side-cart-container'>
		<div class='side-cart' >
		{#if selected}
			<div class='side-cart-inner' transition:fly="{{ x: 250, duration: 1000 }}">
				<h2>{selected.name}</h2>
				<p>${selected.price / 100}</p>
			</div>
		{/if}
		</div>	
	</div>
