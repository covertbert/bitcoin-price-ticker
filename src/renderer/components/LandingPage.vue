<template lang="pug">
	div.ticker-window
		img.ticker-window__logo(:src="bitcoinLogo", @click="getPrice('clicked')", :class="{ 'ticker-window__logo--spin' :  logoIsSpinning}")
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
        bitcoinPrice: 0,
        logoIsSpinning: false
      }
    },
    components: {},
    methods: {
      getPrice (clicked) {
        if (clicked) {
          this.logoIsSpinning = true
          setInterval(() => {
            this.logoIsSpinning = false
          }, 1000)
        }
        axios.get('https://api.coindesk.com/v1/bpi/currentprice.json')
          .then((data) => {
            const rawPrice = data.data.bpi.USD.rate
            this.bitcoinPrice = rawPrice.split('.')[0]
          })
      }
    },
    mounted () {
      this.getPrice()
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
		width: 100vw;
		height: 100vh;
		background-color: black;
	}

	.ticker-window__logo {
		height: 70%;
		flex: 1;

		&--spin {
			animation: rotateLogo 1s forwards;
		}
	}

	.ticker-window__price {
		color: white;
		flex: 2;
		font-family: 'Inconsolata', monospace;
		font-size: 90px;
		font-weight: 600;
		text-align: center;
	}

	@keyframes rotateLogo {
		from {
			transform: rotate(0deg);
		}
		to {
			transform: rotate(360deg);
		}
	}
</style>
