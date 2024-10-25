<template>
  <div class="app">
    <img alt="Vue logo"
         src="./assets/ton_ai_logo.png">
    <!-- <HelloWorld msg="Welcome to Your Vue.js App" /> -->
    <!-- 使用 TonExchangeBannerWrapper 组件 -->
    <div class="banner-wrapper">
      <TonExchangeBannerWrapper :exchangeId="exchangeId" />
    </div>
    <!-- 添加 Show Ad Popup 按钮 -->
    <button @click="showAdPopup">Show Ad Popup</button>
  </div>
</template>

<script>
// import HelloWorld from './components/HelloWorld.vue'
import TonExchangeBannerWrapper from './components/TonExchangeBannerWrapper.vue'

export default {
  name: 'App',
  components: {
    // HelloWorld,
    TonExchangeBannerWrapper
  },
  data() {
    return {
      // exchangeId: '670a6c94dd7fcddb8deacfbe' // 替换为实际的 exchangeId
      exchangeId: '66f65d2d2ec147ec042aa504'
    }
  },
  mounted() {
    this.initTonAd()
  },
  methods: {
    initTonAd() {
      console.log('initTonAd')
      if (typeof window.TonAISdk === 'object') {
        this.runTonAdInit()
      } else {
        this.loadTonAdSDK()
      }
    },
    runTonAdInit() {
      try {
        // const appKey = 'qZCgcwmuo9NEqLJJl3NSVAV8qPjnpH'
        const appKey = 'moJHPwHiGpSP7Lrz88xY1IAXXamF90'

        const res = window.TonAISdk.TonAdInit({ appKey: appKey, debug: true })
        console.log('TonAdInit 初始化结果:', res)
      } catch (error) {
        console.error('TonAdInit 初始化失败:', error)
      }
    },
    loadTonAdSDK() {
      const script = document.createElement('script')
      script.src = 'https://cdn.jsdelivr.net/npm/ton-ai-sdk@2.1.8/dist/index.umd.js'
      script.onload = () => {
        console.log('TON AI SDK 加载成功')
        setTimeout(() => {
          if (typeof window.TonAISdk === 'object') {
            this.runTonAdInit()
          } else {
            console.error('TonAdInit 函数仍未找到，请检查SDK是否正确导出该函数')
          }
        }, 100)
      }
      script.onerror = () => {
        console.error('TON AI SDK 加载失败')
      }
      document.head.appendChild(script)
    },
    showAdPopup() {
      // 触发广告弹窗
      if (window.TonAISdk && window.TonAISdk.TonAdPopupShow) {
        window.TonAISdk.TonAdPopupShow({ blockId: '66f65d8b2ec147ec042aa530' }) // 替换为实际的 blockId
      } else {
        console.error('TonAdPopupShow 函数未找到，请确保 SDK 已正确加载')
      }
    }
  }
}
</script>

<style>
.app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  display: flex;
  flex-direction: column;
  align-items: center;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

.banner-wrapper {
  display: flex;
  justify-content: center;
  width: 100%;
  margin-top: 20px;
}

button {
  margin-top: 20px;
  padding: 10px 20px;
  font-size: 16px;
  cursor: pointer;
}
</style>
