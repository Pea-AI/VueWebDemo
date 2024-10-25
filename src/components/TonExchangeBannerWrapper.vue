<template>
  <div class="wrapper">
    <span class="banner-title">Ton Exchange Banner</span>
    <div ref="bannerContainer"
         class="banner-container"></div>

  </div>
</template>

<script>
export default {
  name: 'TonExchangeBannerWrapper',
  props: {
    exchangeId: {
      type: String,
      required: true
    }
  },
  mounted() {
    setTimeout(() => {
      this.renderBanner()
    }, 1000)
  },
  methods: {
    renderBanner() {
      if (window.TonAISdk && window.TonAISdk.TonExchangeBanner && window.React && window.ReactDOM) {
        const TonExchangeBanner = window.TonAISdk.TonExchangeBanner
        const React = window.React
        const ReactDOM = window.ReactDOM

        const bannerElement = React.createElement(TonExchangeBanner, {
          exchangeId: this.exchangeId
        })

        ReactDOM.render(bannerElement, this.$refs.bannerContainer)
      } else {
        console.error('TonExchangeBanner 组件或 React/ReactDOM 未找到')
      }
    }
  },
  beforeUnmount() {
    if (window.ReactDOM && this.$refs.bannerContainer) {
      window.ReactDOM.unmountComponentAtNode(this.$refs.bannerContainer)
    }
  }
}
</script>

<style scoped>
.wrapper {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 10px;
}

.banner-title {
  font-size: 18px;
  font-weight: bold;
}

.banner-container {
  width: 500px;
  padding-top: 100px;
  border: 1px solid #000;
  position: relative;
  border-radius: 10px;
  overflow: clip;
  display: flex;
  justify-content: center;
  align-items: center;
}

.banner-container>* {
  position: absolute;
  top: 50%;
  left: 50%;
  width: 100%;
  height: 100%;
  transform: translate(-50%, -50%);
  display: flex;
  justify-content: center;
  align-items: center;
}
</style>
