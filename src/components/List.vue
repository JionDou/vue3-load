<script>
import { ref, reactive, onMounted } from 'vue'
import { throttle } from 'lodash-es'

export default {
  setup() {
    const list = reactive([{ background: 'rgb(233,32,38)' }])
    let loading = false
    const loadingText = ref("请稍等，加载中...")

    function getList(num) {
      loading = true
      setTimeout(() => {
        const colors = []
        for (let i = 0; i < num; i++) {
          colors.push({
            background:
              'rgb(' +
              Math.floor(Math.random() * 256) +
              ',' +
              Math.floor(Math.random() * 256) +
              ',' +
              Math.floor(Math.random() * 256) +
              ')',
          })
        }
        list.push(...colors)
        loading = false
      }, 5000)
    }

    onMounted(() => {
      getList(10)

      window.addEventListener(
        'scroll',
        throttle(() => {
          const scrollTop =
            document.documentElement.scrollTop || document.body.scrollTop
          const scrollHeight =
            document.documentElement.scrollHeight || document.body.scrollHeight
          const clientHeight =
            document.documentElement.clientHeight || document.body.clientHeight

          if (scrollTop + clientHeight >= scrollHeight / 2 && list.length < 50) {
            getList(10)
          }
        }, 300)
      )
    })

    return { list, loading, loadingText }
  },
}
</script>

<template>
  <div class="list-container">
    <div v-for="(item, index) in list" :key="index" class="list-item" :style="{ backgroundColor: item.background }">
      {{ index }}
    </div>
    <div v-if="!loading" class="scroll-container__bottom">
      <div class="loading-state">
        <span class="icon rotate">
          <svg t="1651889853940" class="icon" viewBox="0 0 1024 1024" version="1.1" xmlns="http://www.w3.org/2000/svg"
            p-id="1937" width="48" height="48">
            <path
              d="M533.333333 682.666667v192h-64v-192h64z m-175.317333-72.618667l45.269333 45.269333-135.765333 135.744-45.248-45.226666 135.744-135.786667z m286.634667 0l135.744 135.765333-45.226667 45.248-135.786667-135.744 45.269334-45.269333zM330.666667 480v64h-192v-64h192z m533.333333 0v64h-192v-64h192z m-128.853333-247.061333l45.248 45.226666-135.744 135.786667-45.269334-45.269333 135.765334-135.744z m-467.626667 0l135.765333 135.744-45.269333 45.269333-135.744-135.765333 45.226667-45.248zM533.333333 149.333333v192h-64V149.333333h64z"
              p-id="1938" />
          </svg>
        </span>
        <p class="loading-text">
          {{ loadingText }}
        </p>
      </div>
    </div>
  </div>
</template>

<style scoped>
.list-container {
  width: 100vw;
  height: 500px;
}

.list-item {
  width: 100%;
  height: 500px;
  display: flex;
  justify-content: center;
  align-items: center;
  font-size: 32px;
  color: #fff;
}

.scroll-container__bottom {
  height: 60px;
  display: flex;
  justify-content: center;
  align-items: center;
}

.loading-state {
  display: flex;
  justify-content: center;
  align-items: center;
  color: #cccccc;
  transition: .3s ease-in-out;
}

.loading-state.cursor {
  cursor: pointer;
}

.loading-state.cursor:hover,
.loading-state.cursor:active {
  color: #666666;
}

.loading-text {
  padding-left: 5px;
  font-size: 16px;
  color: currentColor;
}

.icon {
  display: block;
  width: 24px;
  height: 24px;
}

.icon.rotate {
  animation: rotate 3s linear infinite;
}

@keyframes rotate {
  0% {
    transform: rotate(0deg);
  }

  100% {
    transform: rotate(360deg);
  }
}

.icon svg {
  width: 24px;
  height: 24px;
  fill: currentColor;
}
</style>
