<template>
  <div class="container">
    <pic
      v-if="showPic"
      class="pic-container"
      :style="{ opacity: opacity }"
    ></pic>
    <div
      v-if="!boomCir"
      :class="['water-drop', isCir ? 'cir-water' : 'none-rule-water']"
      :style="{ top: positionTop + 'px' }"
    ></div>
    <div
      v-else-if="cir"
      style="
        width: 100%;
        height: 100%;
        display: flex;
        align-items: center;
        justify-content: center;
      "
    >
      <div :class="['water-drop', 'boom-cir']"></div>
    </div>
  </div>
</template>

<script lang="ts">
import { Options, Vue } from "vue-class-component";
import pic from "./pic.vue";
@Options({
  components: {
    pic
  }
})
export default class Home extends Vue {
  positionTop: number = -120;
  speed: number = 1;
  timer: any;
  isCir: boolean = false;
  boomCir: boolean = false;
  showPic: boolean = false;
  cir: boolean = true;
  opacity: number = 0;
  mounted() {
    this.timer = setInterval(() => {
      this.positionTop = this.positionTop += this.speed;
      if (this.positionTop % 100 === 0) {
        this.speed += 1;
      }
      if (window.innerHeight - 180 - this.positionTop <= 0) {
        window.clearInterval(this.timer);
        this.isCir = true;
        this.cirToTop().then(() => {
          this.cirBoom();
        });
      }
    }, 10);
  }
  cirToTop(): any {
    return new Promise((resolve: any, reject) => {
      this.timer = setInterval(() => {
        this.positionTop -= this.speed;
        if (window.innerHeight / 2 >= this.positionTop + 90) {
          window.clearInterval(this.timer);
          resolve();
        }
      }, 10);
    });
  }
  cirBoom(): any {
    this.boomCir = true;
    setTimeout(() => {
      this.cir = false;
    }, 3800);
    setTimeout(() => {
      this.showPic = true;
      setTimeout(() => {
        this.opacity = 1;
      }, 1000);
    }, 1000);
  }
}
</script>
<style scoped>
.container {
  width: 100%;
  height: 100%;
  background: rgba(40, 134, 241, 0.925);
  overflow: hidden;
}
.water-drop {
  width: 180px;
  height: 180px;
  margin: 0 auto;
  position: absolute;
  left: 50%;
  transform: translateX(-50%) rotate(46deg);
  box-sizing: border-box;
  box-shadow: inset 10px 20px 30px rgba(0, 0, 0, 0.5),
    10px 10px 20px rgba(0, 0, 0, 0.3), 15px 15px 30px rgba(0, 0, 0, 0.05),
    inset -10px -10px 15px rgba(255, 255, 254, 0.83);
}
.none-rule-water {
  border-radius: 30% 70% 70% 30% / 30% 35% 65% 70%;
  /*使用关键帧  watermove  9s播放  匀速 无限循环*/
  animation: watermove 9s linear infinite;
}
.cir-water {
  /* animation: cirWater 1s; */
  border-radius: 50%;
  /* transition: all 1s; */
}

.boom-cir {
  /* transform: scaleZ(2); */
  border-radius: 50%;
  /* transform: scale(1.5); */
  /* transform-origin: right; */
  transition: all 2s;
  animation: cirBoom 4s linear;
}
@keyframes cirBoom {
  0% {
    width: 180px;
    height: 180px;
    opacity: 0;
  }
  100% {
    width: 2000px;
    height: 2000px;
    opacity: 1;
  }
  /* 100%{
    width: 180px;
    height: 180px;
    opacity: 0;
  } */
}
@keyframes cirWater {
  form {
    border-radius: 30% 70% 70% 30% / 30% 35% 65% 70%;
  }
  to {
    border-radius: 50%;
  }
}
@keyframes watermove {
  20% {
    border-radius: 30% 70% 53% 47% / 28% 44% 56% 72%;
  }

  40% {
    border-radius: 30% 70% 39% 61% / 34% 39% 61% 66%;
  }

  60% {
    border-radius: 25% 75% 45% 55% / 40% 55% 45% 60%;
  }

  80% {
    border-radius: 28% 72% 31% 69% / 32% 39% 61% 68%;
  }
}
.pic-container {
  position: fixed;
  width: 100%;
  height: 100%;
  opacity: 0;
  /* transition: all 1s linear; */
  animation: fade 1s;
}
@keyframes fade {
  form {
    opacity: 0;
  }
  to {
    opacity: 1;
  }
}
</style>
