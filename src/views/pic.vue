<template>
  <div class="pic">
    <div
      class="pic-container"
      :style="{ transform: 'scale(' + this.scaleS + ')' }"
    >
      <!-- <swiper
        :autoplay="swiper_options.autoplay"
        :loop="swiper_options.loop"
        :speed="swiper_options.speed"
        :pagination="swiper_options.pagination"
      >
        <swiper-slide v-for="item in picList" :key="item">
          <div class="picture-list">
            <img :src="require(`../assets/${item}.png`)" />
          </div>
        </swiper-slide>
      </swiper> -->
      <swiper
        :autoplay="swiper_options.autoplay"
        :loop="swiper_options.loop"
        :speed="swiper_options.speed"
        :pagination="swiper_options.pagination"
        :navigation="swiper_options.navigation"
        :spaceBetween="swiper_options.spaceBetween"
        :coverflowEffect="swiper_options.coverflowEffect"
        :centeredSlides="swiper_options.centeredSlides"
        :slidesPerView="swiper_options.slidesPerView"
        class="swiper"
        effect="coverflow"
      >
        <swiper-slide v-for="item in 4" :key="item">
          <div class="picture-list">
            <img :src="require(`../assets/${item + 1}.jpg`)" />
          </div>
        </swiper-slide>
        <!-- <swiper-slide
          ><img class="my_swiper_imgs" src="../assets/1.png" alt=""
        /></swiper-slide>
        <swiper-slide
          ><img class="my_swiper_imgs" src="../assets/2.png" alt=""
        /></swiper-slide>
        <swiper-slide
          ><img class="my_swiper_imgs" src="../assets/3.png" alt=""
        /></swiper-slide> -->
      </swiper>
    </div>
  </div>
</template>
<script lang="ts">
import SwiperCore, {
  Autoplay,
  Pagination,
  EffectCoverflow,
  Navigation
} from "swiper";
import { Swiper, SwiperSlide } from "swiper/vue";
require("swiper/swiper.min.css");
require("swiper/components/pagination/pagination.less");
require("swiper/components/navigation/navigation.less");
SwiperCore.use([Autoplay, Pagination, EffectCoverflow, Navigation]);
import { reactive } from "vue";
export default {
  name: "MySwiper",
  components: {
    Swiper,
    SwiperSlide
  },
  setup() {
    let swiper_options = reactive({
      autoplay: {
        disableOnInteraction: false, // 鼠标滑动后继续自动播放
        delay: 4000 //4秒切换一次
      },
      speed: 500, //切换过渡速度
      loop: true,
      slidesPerView: "auto", //解决最后一张切换到第一张中间的空白
      centeredSlides: true, //设置slide居中
      spaceBetween: 20,
      coverflowEffect: {
        // rotate: 0, //slide做3d旋转时Y轴的旋转角度。默认50。
        stretch: 50, //每个slide之间的拉伸值（距离），越大slide靠得越紧。 默认0。
        depth: 100, //slide的位置深度。值越大z轴距离越远，看起来越小。 默认100。
        modifier: 1 //depth和rotate和stretch的倍率，相当于            depth*modifier、rotate*modifier、stretch*modifier，值越大这三个参数的效果越明显。默认1。
        // slideShadows: false, //开启slide阴影。默认 true。
      },
      navigation: {
        nextElRef: ".swiper-button-next",
        prevElRef: ".swiper-button-prev"
      },
      pagination: {
        clickable: true,
        dynamicBullets: true
      }
    });
    // 将swiper_options返回给模板中的swiper组件使用
    return { swiper_options };
  }
};
</script>
<style scoped lang="less">
.pic {
  width: 100%;
  height: 100%;
  display: flex;
  align-items: center;
  justify-content: center;
}
/deep/.swiper-button-next {
  right: -30px;
  color: #ffffff;
}
/deep/.swiper-button-prev {
  left: -30px;
  color: #ffffff;
}
.pic-container:hover {
  /deep/.swiper-button-next {
    right: 10px;
    transition: all 0.3s;
  }
  /deep/.swiper-button-prev {
    left: 10px;
    transition: all 0.3s;
  }
}
.pic-container {
  width: 90%;
  transition: all 1s;
  height: 100%;
  overflow: hidden;
  display: flex;
  align-items: center;
  /* animation: scaleBig 1s linear; */
}
.picture-list {
  height: 100%;
}
.picture-list img {
  max-height: 100%;
  max-width: 100%;
}
@keyframes scaleBig {
  form {
    transform: scale(1);
  }
  to {
    transform: scale(1.3);
  }
}
/deep/.swiper-slide-shadow-left {
  background-image: none;
}
/deep/.swiper-slide-shadow-right {
  background-image: none;
}
/deep/.swiper-pagination-bullet {
  background: #333;
  opacity: 1;
}
/deep/.swiper-pagination-bullet-active {
  background: #ffffff;
}
</style>
