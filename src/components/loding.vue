<template>
  <div>
    <div class="pageLoading">
      <div class="chunk">
        <div class="loading">
          <div class="bar">
          </div>
        </div>
        <div class="right animate__animated" :class="{
            'changeBg':isok,
            'animate__rubberBand':isok
        }">
          <span class="qz" :class="{
            'ok':isok ,
            }"></span>
        </div>
        <div class="left"></div>
        <div v-if="!isok" class="xj" :style="{
          'margin-left':`${per}px`
        }"></div>
      </div>
    </div>
  </div>
</template>
<script>
import Praise from "./praise.vue";
export default {
  data() {
    return {
      isok: false,
      per: 0,
    };
  },
  components: {
    Praise,
  },
  mounted() {
    this.per = 0;
    var bar = document.getElementsByClassName("bar")[0];
    var loadingPage = document.getElementsByClassName("pageLoading")[0];
    var timer = setInterval(() => {
      bar.style.width = this.per + "px";
      this.per += 1;
      if (this.per > 200) {
        this.isok = true;
        clearInterval(timer);
        const timer1 = setTimeout(() => {
          loadingPage.classList.add("complate");
          clearTimeout(timer1)
          const timer2 = setTimeout(() => {
            clearTimeout(timer2)
            loadingPage.classList.add("close");
          }, 1000);
        }, 1000);
      }
    }, 10);
  },
};
</script>
<style lang="scss" scoped>
/* 第一页 */
.pageLoading {
  position: fixed;
  width: 100%;
  height: 100%;
  left: 0;
  top: 0;
  background-color: #0c4475;
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
  transition: opacity 0.5s 0.5s;
  z-index: 9999;
}
.pageLoading .chunk {
  position: relative;
  // margin-top: 10px;
  .xj {
    width: 20px;
    height: 20px;
    background-image: url("../assets/xj.png");
    background-size: 100%;
    background-repeat: no-repeat;
    position: absolute;
    left: -10px;
    top: -20px;
  }
  .right {
    right: -55px;
    top: -35px;
    position: absolute;
    background-image: url("../assets/yx.png");
    background-repeat: no-repeat;
    background-size: 100%;
    height: 50px;
    width: 50px;
    &.changeBg {
      background-image: url("../assets/ysx.png") !important;
    }
    .qz {
      position: absolute;
      left: 28px;
      top: 4px;
      width: 14px;
      height: 20px;
      background-size: 100%;
      background-repeat: no-repeat;
      background-image: url("../assets/qizi.png");
      transform: rotate(90deg);
      transform-origin: 30% 80%;
      &.ok {
        animation: ok 1s linear forwards;
      }
    }
  }
  .left {
    background-image: url("../assets/ls.png");
    position: absolute;
    left: -50px;
    top: -36px;
    background-repeat: no-repeat;
    background-size: 100%;
    height: 50px;
    width: 50px;
  }
  .loading {
    width: 200px;
    height: 8px;
    border-radius: 5px;
    background-color: #fff;
    overflow: hidden;
  }
  .bar {
    background-color: #e55a54;
    width: 0%;
    height: 100%;
    // animation: loading 3s linear forwards;
    position: relative;
  }
}
.pageLoading.complate {
  opacity: 0;
}
.pageLoading.complate.close {
  z-index: -1;
}

@keyframes loading {
  0% {
    width: 0%;
  }
  100% {
    width: 100%;
  }
}

@keyframes ok {
  0% {
    transform: rotate(90deg);
  }
  100% {
    transform: rotate(0deg);
  }
}
</style>