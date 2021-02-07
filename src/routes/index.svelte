<script>
	import images from '../data.js';
	import SideCart from '../components/SideCart.svelte'
	import Canvas from '../components/Canvas.svelte'

	let hints = {};
	let callbacks = {};
	let loading = null;

	const randomOrder = toString(Math.floor(10000 * Math.random()));
	$: cart = {
		order_reference : randomOrder,
		total_amount: 100,
		currency: "USD",
		items: [],
		discounts: [],
		metadata: {}
	}

	// variables defined with $: are reactive. Similar to stateful variable in React
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
					image_url: item.url
			}
			cart.items.push(tmp)
			if (typeof BoltCheckout !== 'undefined') {
				BoltCheckout.configure(cart, hints, callbacks, {floatingButtonMode: 'show_cart'})
			}
		}	
	}

	$: selected = null;

	function load(image) {
		selected = image;
		loading = null;
	}
</script>

<svelte:head>
	<title>Cat PosterS</title>
</svelte:head>

<Canvas 
	selected={selected}
	images={images}
	load={load}
	updateCart={updateCart}
/>
<SideCart {selected} />


