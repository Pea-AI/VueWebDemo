<template>
  <div class="app">
    <img alt="Vue logo"
         src="./assets/ton_ai_logo.png">
    <!-- 使用 TonExchangeBannerWrapper 组件 -->
    <!-- <div class="banner-wrapper">
      <TonExchangeBannerWrapper :exchangeId="exchangeId" />
    </div> -->
    <!-- 添加 Show Ad Popup 按钮 -->
    <button @click="showAdPopup">Show Ad Popup</button>
    <!-- 添加 GetMulti Ad 按钮 -->
    <button @click="getMultiAd">GetMulti Ad</button>
    <button @click="getMultiExchangeAd">GetMulti Exchange Ad</button>
    <!-- 增加一个 layout 用来展示 getMultiAd 返回的json 数据 -->
    <div class="layout">
      <pre>{{ multiAdData ? JSON.stringify(multiAdData, null, 2) : '暂无数据' }}</pre>
    </div>
  </div>
</template>

<script>
// import TonExchangeBannerWrapper from './components/TonExchangeBannerWrapper.vue'
// import 'ton-ai-sdk/dist/index.css'
import 'ton-ai-sdk/dist/index.esm.css'
import { TonAdInit, TonAdPopupShow, GetMultiTonAd } from 'ton-ai-sdk'

export default {
  name: 'App',
  components: {
    // HelloWorld,
    // TonExchangeBannerWrapper
  },
  data() {
    return {
      exchangeId: '670a6c94dd7fcddb8deacfbe',
      multiAdData: null
    }
  },
  mounted() {
    this.initTonAd()
  },
  methods: {
    initTonAd() {
      console.log('initTonAd')
      TonAdInit({ appId: '670777796d32dfbd6351ecc1', debug: true })
    },

    showAdPopup() {
      // 触发广告弹窗
      TonAdPopupShow({ blockId: '6718a53bb877e79f84e68bd5' })

    },
    async getMultiAd() {
      try {
        const result = await GetMultiTonAd(
          '6718a53bb877e79f84e68bd5'
        )
        this.multiAdData = result
        console.log('multiAdData', this.multiAdData)
      } catch (error) {
        console.error('获取广告数据失败:', error)
        this.multiAdData = { error: '获取数据失败' }
      }

    },
    async getMultiExchangeAd() {
      const result = await GetMultiExchangeAd('670a6c94dd7fcddb8deacfbe')
      console.log('multiExchangeAdData', result)
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
  color: #fff;
  font-weight: 600;
  background-color: #007bff72;
  border-radius: 10px;
  cursor: pointer;
}

.layout {
  margin-top: 20px;
  width: 800px;
  height: 600px;
  text-align: left;
  border: 1px solid #000;
  background-color: #f0f0f0;
  padding: 10px;
  border-radius: 10px;
  overflow: auto;
}
</style>
