<script>
	import {fly} from 'svelte/transition';
    export let selected
</script>

<style>
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

