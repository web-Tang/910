
<template>
  <div class="main">
    <div class="envelope" :class="{'active':open}">
      <div class="top"></div>
      <div class="heart" @click="handleOpen"></div>
      <div class="card">
        <div :style="{'font-size':fontSize+'px'}">
          <div class="letter" @click="openMaxLetter">
            Ming
            <br>
            <br>
            &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
            夜にはいつも寒いよね、でも、手を繋いでいると、暖かくなるよ！
            <br>
            &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
            どんなに寒い夜も、君と二人でいれば、ちっとも寒くない！
            <br>
            <br>
            <p style="float:right; display:block;">Hao</p>
          </div>
        </div>
      </div>
      <canvas ref="canvas"></canvas>
    </div>
    <span v-show="playshow" class="music" :class="{
      'play':play
    }" @click="toggleMusic">
      <audio id="audio" autoplay="autoplay">
        <source src="http://mat1.gtimg.com/news/2015/love/FadeAway.mp3" type="audio/mp3" />
      </audio>
    </span>
    <div v-show="isShowMaxletter" class="max-letter">
      <div class="content">内容</div>
    </div>
    <loding />
  </div>

</template>

<script >
// 初次进入loading，点赞溅射效果
// 信封初次打开后就不能再打开
// 音乐开启/暂停
// 信由图片替代
// 整体颜色重新设计
// 信封样式设计

// 后续:
// 增加全屏滚动
// 增加md展示内容
import { ref, watch } from "vue";
import confetti from "canvas-confetti";
import { list } from "../assets/content.json";
import loding from "./loding.vue";

const open = ref(false);
const btnEnd = ref(false);
const content = ref("");
const end = ref(true);
const fontSize = ref(12);
const canvas = ref(null);

const wordShow = ref(false);

let bomm = confetti.create(canvas.value, { resize: true });

export default {
  components: {
    loding,
  },
  data() {
    return {
      open: "",
      end: "",
      content: "",
      word: "",
      wordShow,
      fontSize,
      msg: "",
      audio: null,
      isShowMaxletter: false,
      play: false,
      playshow:false
    };
  },
  mounted() {
    this.audio = document.getElementById("audio");
    this.open = open;
    this.end = end;
    watch(open, (v) => {
      if (!v) return;
      this.play = true;
      end.value = false;
      let msg = list[~~(Math.random() * list.length)];
      this.msg = msg;
      fontSize.value = msg.fontSize;
      setTimeout(() => bomm(), 1000);
      setTimeout(() => this.contentLog(msg.text), 2222);
    });

    watch(end, (v) => {
      if (v) return;
    });
  },
  methods: {
    openMaxLetter() {
      this.isShowMaxletter = true;
    },
    toggleMusic() {
      this.play = !this.play;
      this.play ? this.audio.play() : this.audio.pause();
    },
    contentLog() {
      // content.value = this.msg;
      // end.value = true;
    },
    handleOpen() {
      if (!end.value) return;
      this.playshow = true
      this.audio.play();

      wordShow.value = false;
      if (end.value && !open.value) {
        // bgm.value.currentTime = 0;
        // bgm.value.play();
        open.value = true;
      } else if (end.value && open.value && btnEnd.value) {
        end.value = true;
        open.value = false;
        btnEnd.value = false;
        content.value = "";
      }
    },
  },
};
</script>

<style scoped lang="scss">
$heart-color: rgb(248, 82, 82);
$envelope-color: rgb(252, 240, 175);
$in-color: rgb(252, 252, 198);
$msg-color: rgb(192, 82, 82);

.max-letter {
  position: fixed;
  width: 100%;
  height: 100%;
  top: 0px;
  left: 0px;
  background-color: rgba(0, 0, 0, 0.5);
  color: #fff0c9;
  padding: 10%;
  .content {
    background-color: #837362;
    width: 100%;
    height: 100%;
  }
}

.letter {
  cursor: pointer;
  color: #837362;
}

.music {
  position: absolute;
  right: 10px;
  top: 10px;
  width: 25px;
  height: 25px;
  background-image: url("data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAADcAAAA3CAMAAACfBSJ0AAAAh1BMVEUAAAD///////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////9qkf8RAAAALHRSTlMA6j0E4S2vQO/nFWs2Dgga+VMo7M25h3ZG02Vbmnww87NNP96oI8CMb5Lg1UAzAj8AAAKLSURBVEjHndbbkqMgEADQRpSAGsX7XeMmMcmO//99WzqzEgWMNf2QB6tOEZqGblCFHaSkQHzkqLikgQ2Hws9iemmC1saA7TZoLjTO/I/KJVF9wqtP+FRHxN1VYXItPcV3r7wmoVZ5KXMwqAM7LPU0ixVkLwU2KZRL5siB/XBQLn+smAGfwkCVxEwLPjvKNjA3wwMMnaz1X70j9xCbfu5vBxCXBxmAE4vjSMke8PI0Nmc2B0mXg2Pac8NGM3Dz5owLA5v9T0XiqI2bJR26lSHAeVwYgJP8lLKJZWQ9L5SSp/WT7lEwwOZ3Eom0XHBDXZK5eElJNK7qZk6HT6V6/fsyJiNYs3Iene5jVsM2TOs9MzXnbOWgzgAgPqmcyAzlj/C8dqcYwI+wwonMVFMmNw4jGwICsgvKn8zMxy05IAGkjcLxoTGwKC7JNQ8gvcK1q5qUXX+BwlI4SzClswpAtsYJJjufAccaJ5jszh2MoHGCyQ5GzXqCadbT7U8w3OZ5L+1Pk0/Byoher1cpn+rzqxZWoVx5fsp6YfS0XKleXS+q+jTGcnkgKYCyPrf3oa9vA+fLnkP0nYdOug8QGyDixh51V9PFYerOyRnWfyie7vtLfHCKs4H+vN/37OsO4Eb5yr0ymC6ueF+KfmLvDr+6Ie7KFfMif/Oe8X5iws2pKatNaThk+35GdGIeV9SQ/H6K99rgybynYb/tJpv+YKDArFsrpa3WiP4g+tGUEv9GebLLgKSr/jezDyH6n+i3B5mL7qv+zughZqF8O4ZYB1hoVr+ZX1xW/WpeinLNfObvKF8znwF4j+ipnQef0cMDXbQ782f7Yd6linmXEvfwfO2fAfTztX6eZ3Sk7Es3z/8D0fgrl5NXHdYAAAAASUVORK5CYII=");

  background-size: 25px;
  z-index: 99;
}
.music.play {
  background-image: url("data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAADcAAAA3CAMAAACfBSJ0AAAAilBMVEUAAAD////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////2N2iNAAAALXRSTlMA6y3h5gVsDgg2snYaPe/UFqtTKPK4r4Z9ZUxFMAP3WpokE97NiyzOv2mjksf96K+aAAACWUlEQVRIx52W6a6CMBCFp9pi2cq+L4LiLu//ejdgIkJL4XJ+GGnypfQM0zkg0smNHAu1bYss7eKeYJUMvfG12M1tAsTO3VjzG91YpNQQVSYZLRGzQqEqpbzgnFDgRZNz4M1SNMIPAmKRB47ozGZWaMO87NASbpkhBeRSUMYvHrEJSzLxkceusKzrFMywB2u0G79qjVRYJxPVPwVoElgrpRnKETkgEc2iNx4ew+hbODxbN2LG9xI/lfanjl8rAnHhClUPfOQ8aoDDDwdKAL3UPREYp2iMacqu+z/hyP5jYsht5zrID3S1AAABB0rY9xujU27/GrcSTUccZV0/6hVw3G7qzIiDSgeAxhRzgzNJfRhzZgNgo0LM9c6EH2cmXIFO4GrAc+5jcEbAgeZCFAu48h4Pzgi4+AJOKuDy7lfCpRpYOwG3W+CuFiB7A2dgKMkG7uBDCxs4aLfut3w+kmdZyp1v0c8EsfP5zPvppFLuiDJx/aJYyu3Tme/FDWVczYqZ79MY90NaOY5TfjkPfXzwuX6A22//PfFF13X25QhTe3Pu4/67df3+GhYU6zQ5n24ZAOpkEL307h3QcL9Y6dSX4lXe334ywigy+vvs8V0pT3zd6+RoT656bXp/4h6gpfj+Hu7P6X1dPfszveWTJeDmg72v8mvk5zLMxh4/j4wnKwMxxs8joLd/zL8b3TJvVVRvnO9b8oSHj1vyi8pjAOmKvJTO5DNDQhmONeMBvSBlNg8q6EJhTrkkf+byo2tMkHeZpq7J16zL18YBij5fs5skX/N5Hvutj2fz/B/LOymzX/xZaQAAAABJRU5ErkJggg==");
  -webkit-animation: rotate 3.82s linear infinite;
  animation: rotate 3.82s linear infinite;
}

@keyframes rotate {
  0% {
    -webkit-transform: rotate(0deg);
  }
  100% {
    -webkit-transform: rotate(360deg);
  }
}
@-webkit-keyframes rotate {
  0% {
    -webkit-transform: rotate(0deg);
  }
  100% {
    -webkit-transform: rotate(360deg);
  }
}

@keyframes wordShow {
  0%,
  100% {
    opacity: 0;
  }
  30%,
  70% {
    opacity: 1;
  }
}
#footer {
  /* width: 100%; */
  position: fixed;
  /* bottom: 100px; */
  top: 10px;
  right: 10px;
}
.sorry {
  position: absolute;
  position: fixed;
  left: 50%;
  top: 50%;
  z-index: 999;
  animation: sorry 1s 6s forwards;
}
@keyframes sorry {
  0% {
    opacity: 1;
  }
  100% {
    opacity: 0;
  }
}

@keyframes showBtns {
  0% {
    opacity: 0;
    transform: translateY(200px);
  }
  100% {
    opacity: 1;
    transform: translateY(0px);
  }
}

.btns {
  position: absolute;
  position: fixed;
  bottom: 15px;
  left: 0;
  right: 0;
  z-index: 1000;
  display: none;
  &.active {
    display: block;
    animation: showBtns 0.8s 1s linear backwards;
  }
  div {
    cursor: pointer;
    width: 360px;
    height: 36px;
    color: #333333;
    display: flex;
    font-weight: bold;
    font-size: 16px;
    align-items: center;
    justify-content: center;
    border: 1px solid #333333;
    background-color: rgba(255, 255, 255, 0.5);
    margin: 0 auto 15px;
    transition: 0.2s all;
    &:active {
      transform: scale(0.95);
    }
  }
}

.envelope {
  &.active {
    transition: 0.8s 1s transform;
    transform: translateY(50px);
    &::before {
      animation: hide 0.2s 1s ease-out;
      animation-fill-mode: forwards;
    }
    &::after {
      animation: afterUp 0.2s 1s ease-out;
      animation-fill-mode: forwards;
    }
    .top {
      transition: 0.5s all;
      transition-delay: 1s;
      transform: rotateX(180deg) translateY(-2px) scaleY(1.5);
    }
    .card {
      animation: show 0.8s 1.5s ease-out;
      animation-fill-mode: forwards;
      z-index: 9;
      p {
        animation: showContent 2.5s 1s ease-out forwards;
      }
    }
    .heart {
      animation: heartBeat 1s;
      animation-fill-mode: forwards;
    }
  }
}

.envelope {
  width: 280px;
  height: 160px;
  background: $in-color;
  position: relative;
  box-shadow: 2px 2px 10px rgba(0, 0, 0, 0.2);
  border-radius: 5px;
  canvas {
    position: absolute;
    left: 0;
    top: 0;
    bottom: 0;
    right: 0;
    z-index: 100;
  }
  // overflow:hidden;
  &::after {
    content: "";
    display: block;
    position: absolute;
    border-width: 80px 140px;
    top: 0;
    border-style: solid;
    border-color: transparent $envelope-color $envelope-color $envelope-color;
    transition: 0.3s all;
    transform: rotateX(0deg);
    transform-origin: 50% 0%;
    z-index: 6;
    border-radius: 5px;
  }
  &::before {
    content: "";
    display: block;
    position: absolute;
    border-width: 80px 140px;
    top: 0;
    border-style: solid;
    border-color: rgba(0, 0, 0, 0.3) transparent transparent transparent;
    transition: 0.3s all;
    transform-origin: 50% 0%;
    z-index: 7;
    filter: blur(2px);
    border-radius: 5px;
  }
  .top {
    position: absolute;
    border-width: 80px 140px;
    top: 0;
    left: 0;
    border-style: solid;
    border-color: $envelope-color transparent transparent transparent;
    transform: rotateX(0deg);
    transform-origin: 50% 0%;
    z-index: 8;
    border-radius: 5px;
  }
}

.card {
  width: 240px;
  height: 180px;
  background-color: white;
  position: absolute;
  z-index: 5;
  left: 50%;
  margin-left: -120px;
  bottom: 20px;
  transition: 0.2s 0.1s all;
  box-shadow: 1px 1px 15px rgba(0, 0, 0, 0.2);
  opacity: 0;
  box-sizing: border-box;
  padding: 5% 6.8%;
  filter: contrast(20);
  p {
    font-size: 12px;
    color: $heart-color;
    font-family: fantasy;
    line-height: 32px;
    -webkit-text-stroke: 1px $heart-color;
    text-shadow: -1px 0 1px #f5e6c2, 0 1px 2px #fff0c9;
  }
}

@keyframes showContent {
  0% {
    filter: blur(12px);
  }
  100% {
    filter: blur(0px);
  }
}

.heart {
  position: absolute;
  width: 15px;
  height: 15px;
  transform: rotate(45deg);
  z-index: 520;
  background-color: $heart-color;
  left: 50%;
  margin-left: -2px;
  top: 75px;
  transition: 0.3s all;
  transform-origin: 50% 0%;
  box-shadow: -3px 3px 16px rgba(0, 0, 0, 0.6);
  cursor: pointer;
  &::before {
    content: "";
    display: block;
    width: 15px;
    height: 15px;
    border-radius: 50%;
    position: absolute;
    background-color: $heart-color;
    left: -8px;
    top: 0px;
  }
  &::after {
    content: "";
    display: block;
    width: 15px;
    height: 15px;
    border-radius: 50%;
    position: absolute;
    background-color: $heart-color;
    right: 0px;
    top: -8px;
  }
}

@keyframes show {
  0% {
    transform: scaleY(0.6) translateY(20px);
    opacity: 0.7;
  }
  100% {
    transform: scaleY(1) translateY(-80px);
    opacity: 1;
  }
}

@keyframes hide {
  0% {
    opacity: 1;
  }
  100% {
    opacity: 0;
  }
}

@keyframes afterUp {
  0% {
    z-index: 8;
  }
  100% {
    z-index: 10;
  }
}

@keyframes heartBeat {
  0% {
    transform: rotate(45deg) scale(1);
  }

  14% {
    transform: rotate(45deg) scale(1.3);
  }

  28% {
    transform: rotate(45deg) scale(1);
  }

  42% {
    transform: rotate(45deg) scale(1.3);
  }
  70% {
    transform: rotate(45deg) scale(1);
  }
  100% {
    transform: rotate(0deg) translateY(5px);
  }
}
</style>
