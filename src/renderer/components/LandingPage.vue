<template lang="pug">
	div.ticker-window
		img.ticker-window__logo(:src="bitcoinLogo")
		p.ticker-window__price(v-text="`$${bitcoinPrice}`")
</template>

<script type="text/babel">
  import axios from 'axios'
  import bitcoinLogo from '../assets/img/bitcoin.svg'
  export default {
    name: 'landing-page',
    data () {
      return {
        bitcoinLogo,
        bitcoinPrice: 0
      }
    },
    components: {},
    methods: {
      getPrice () {
        axios.get('https://api.coindesk.com/v1/bpi/currentprice.json')
          .then((data) => {
            const rawPrice = data.data.bpi.USD.rate
            this.bitcoinPrice = rawPrice.split('.')[0]
          })
      }
    },
    mounted () {
      setInterval(() => {
        this.getPrice()
      }, 10000)
    }
  }
</script>

<style scoped lang="scss" rel="stylesheet/scss">
	@import url('https://fonts.googleapis.com/css?family=Inconsolata');

	.ticker-window {
		position: relative;
		display: flex;
		align-items: center;
		justify-content: center;
		flex-direction: row;
		width: 500px;
		height: 180px;
		background-color: white;
	}

	.ticker-window__logo {
		height: 80%;
		flex: 1;
	}

	.ticker-window__price {
		flex: 2;
		font-family: 'Inconsolata', monospace;
		font-size: 90px;
		font-weight: 600;
	}
</style>
