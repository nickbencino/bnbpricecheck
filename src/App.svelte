<script>
  	import { onMount } from 'svelte';

    onMount(async () => {

      
    });

  const date = async () => {
    const timeString = document.querySelector('.bnbPriceTime').value + ':00.000Z';
    return Math.round(new Date(timeString)/1000);
  };

  const getPrice = async () => {
    const to = await date();
    const from = to-1;
    const res = await fetch(`https://finnhub.io/api/v1/crypto/candle?symbol=BINANCE:BNBUSDT&resolution=1&from=${from}&to=${to}&token=bv7trqf48v6vtp9vnpi0`);
    const price = await res.json();
    return price;
  }

  const fillPrice = async () => {
    const priceData = await getPrice();
    const priceBox = document.querySelector('.price');
    const averagePrice = (priceData.h[0] + priceData.l[0]) / 2;
    priceBox.innerHTML = averagePrice.toFixed(2);
  };
</script>

<main>
  <h1>Check Price of BNB At Specific Date</h1>
	<label for="bnbpricetime">Time to check (in UCT):</label>
  <input type="datetime-local" class="bnbPriceTime" name="bnbpricetime" on:change={fillPrice}>

  <div class="price">

  </div>

</main>

<style>
	main {
		text-align: center;
		padding: 1em;
		max-width: 240px;
		margin: 0 auto;
	}

	h1 {
		color: #ff3e00;
		text-transform: uppercase;
		font-size: 4em;
		font-weight: 100;
	}

  .price {
    font-size: 4rem;
    margin-top: 2rem;
  }

	@media (min-width: 640px) {
		main {
			max-width: none;
		}
	}
</style>