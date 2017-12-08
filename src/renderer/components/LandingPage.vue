<template lang="pug">
	div.ticker-window
		img.ticker-window__logo(:src="bitcoinLogo", @click="getPrice('clicked')")
		p.ticker-window__price(v-text="`$${bitcoinPrice}`")
</template>

<script type="text/babel">
  import axios from 'axios'
  import bitcoinLogo from '../assets/img/bitcoin.svg'
  import anime from 'animejs'
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
      rotateCoin () {
        anime({
          targets: '.ticker-window__logo',
          rotate: '1turn',
          duration: 1000
        })
      },
      getPrice (clicked) {
        if (clicked) {
          this.rotateCoin()
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
	}

	.ticker-window__price {
		color: white;
		flex: 2;
		font-family: 'Inconsolata', monospace;
		font-size: 90px;
		font-weight: 600;
		text-align: center;
	}
</style>
