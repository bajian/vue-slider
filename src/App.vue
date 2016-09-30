<template>
  <div id="app">
    <slider id="swiper_horizontal"
    :pagination-visible="true"
    :slides="slides"
    :repeating="true"
    :auto="5000"
    @slide-change-start="onSlideChangeStart"
    @slide-change-end="onSlideChangeEnd"
    @slide-revert-start="onSlideRevertStart"
    @slide-revert-end="onSlideRevertEnd"
    @slider-move="onSliderMove">

    <div v-for="slide in slides" track-by="$index">
      <a :href="slide.value">
        <img width="350" height="180" :src="slide.image" />
      </a>
    </div>
  </slider>
  <button @click="prependSlide()">prependSlide</button>
  <button @click="appendSlide()">appendSlide</button>

</div>
</template>

<script>
  import slider from './components/vue-slider'

  export default {
    components: {
      slider
    },
    data(){
      return {
        slides: [{
          "title": "鬼畜频道精选",
          "value": "http://www.bilibili.com/topic/v2/phone1513.html",
          "image": "http://i0.hdslb.com/bfs/archive/98548a7be48ab9929f02d2c51c2ab801841e5108.jpg",
          "type": 2,
          "weight": 1,
          "remark": "",
          "hash": "148861b9b07d47a5e9e8f8fa961f6596"
        },
        {
          "title": "逗趣问答一战成神",
          "value": "http://acg.tv/u1ue",
          "image": "http://i0.hdslb.com/bfs/archive/056f5ad71f17a9fbb9581f16735768e3d9c54008.png",
          "type": 2,
          "weight": 2,
          "remark": "",
          "hash": "3e054fa6f2fb4632eb8c7a6203e5201c"
        },
        {
          "title": "阴阳师",
          "value": "#",
          "image": "http://i0.hdslb.com/bfs/archive/cf6153a50cb0f3eb27a5836d90be16d4bce8c6f7.jpg",
          "type": 1,
          "weight": 3,
          "remark": "",
          "hash": "204cc23aa90fbeff5cc98932f4249ab4"
        }]
      }
    },
    methods: {
      onSlideChangeStart: function (currentPage) {
        console.log('onSlideChangeStart', currentPage);
      },
      onSlideChangeEnd: function (currentPage) {
        console.log('onSlideChangeEnd', currentPage);
      },
      onSlideRevertStart: function (currentPage) {
        console.log('onSlideRevertStart', currentPage);
      },
      onSlideRevertEnd: function (currentPage) {
        console.log('onSlideRevertEnd', currentPage);
      },
      onSliderMove: function (offset) {
        console.log('onSliderMove', offset);
      },
      prependSlide: function (slideText) {
        this.slides.unshift({
          "title": "bajian prepend",
          "value": "#",
          "image": "http://i0.hdslb.com/bfs/archive/cf6153a50cb0f3eb27a5836d90be16d4bce8c6f7.jpg",
        });
      },
      appendSlide: function (slideText) {
        this.slides.push({
          "title": "bajian append",
          "value": "#",
          "image": "http://i0.hdslb.com/bfs/archive/cf6153a50cb0f3eb27a5836d90be16d4bce8c6f7.jpg",
        });
      },
      removeSlide: function () {

      }
    }
  }
</script>

<style>
  html {
    height: 100%;
  }

  body {
    height: 100%;
    max-width: 500px;
    margin: 0 auto
  }

  #app {
    color: #2c3e50;
    max-width: 600px;
    font-family: Source Sans Pro, Helvetica, sans-serif;
    text-align: center;
    width: 100%;
    margin: 0 auto
  }

  .swiper {
    height: 180px;
    margin-bottom: 80px;
    width: 350px;
    margin: 0 auto

  }
  /*change the pagination u like */
  .swiper-pagination-bullet.active {
    background: #FB7299 !important;
  }

</style>
