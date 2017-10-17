<template>
  <div id="app">
    <slider
    :pagination-visible="true"
    :slides="slides"
    :repeating="true"
    :auto="5000"
    @slide-change-start="onSlideChangeStart"
    @slide-change-end="onSlideChangeEnd"
    @slide-revert-start="onSlideRevertStart"
    @slide-revert-end="onSlideRevertEnd"
    @slider-move="onSliderMove">

    <div v-for="(slide,index) in slides" :key="index">
      <a :href="slide.value">
        <img width="100%" height="150" :src="slide.image" />
      </a>
    </div>
  </slider>
  <button @click="prependSlide()">prependSlide</button>
  <button @click="appendSlide()">appendSlide</button>

  <hr/>
  <slider
  :pagination-visible="true"
  :slides="slides"
  :auto="0">

  <div style="position: relative" v-for="(slide,index) in slides" :key="index">
    <a :href="slide.value">
      <img width="350" height="180" :src="slide.image" />
      <span class="title-mask" v-if="slide.title">{{slide.title}}</span>
    </a>
  </div>
</slider>
<h4>custom style,not repeating,not auto</h4>

<hr/>
<div style="position: relative">
  <slider
  ref="test_prev_next"
  :pagination-visible="true"
  :repeating="true"
  slide-container-class="custom-slide-container-class"
  pagination-container-class="custom-pagination-container-class"
  pagination-class="custom-pagination-class"
  :slides="slides">
  <div style="position: relative" v-for="(slide,index) in slides" :key="index">
    <a :href="slide.value">
      <img width="100%" height="180" :src="slide.image" />
      <span class="title-mask" v-if="slide.title">{{slide.title}}</span>
    </a>
  </div>
</slider>
<span @click="prev()" class="button-prev">&#60;</span>
<span @click="next()" class="button-next">&#62;</span>
</div>
<h4>custom previous/next button、custom container-class & pagination-class & pagination-container-class</h4>
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
          "image": "https://ss0.bdstatic.com/94oJfD_bAAcT8t7mm9GUKT-xh_/timg?image&quality=100&size=b4000_4000&sec=1507515098&di=852ed69d68e5869766abef5d0f2e5371&src=http://pic33.nipic.com/20130907/13534366_092511672176_2.jpg",
          "type": 2,
          "weight": 1,
          "remark": "",
          "hash": "148861b9b07d47a5e9e8f8fa961f6596"
        },
        {
          "title": "逗趣问答一战成神",
          "value": "http://acg.tv/u1ue",
          "image": "https://ss1.bdstatic.com/70cFuXSh_Q1YnxGkpoWK1HF6hhy/it/u=2856723681,2411189826&fm=27&gp=0.jpg",
          "type": 2,
          "weight": 2,
          "remark": "",
          "hash": "3e054fa6f2fb4632eb8c7a6203e5201c"
        },
        {
          "title": "阴阳师",
          "value": "#",
          "image": "https://ss1.bdstatic.com/70cFuXSh_Q1YnxGkpoWK1HF6hhy/it/u=3439730901,2621203596&fm=27&gp=0.jpg",
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
          "image": "https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1508121479&di=7549cbb34fbb872d9c9a2986993bb9de&imgtype=jpg&er=1&src=http%3A%2F%2Fimg.taopic.com%2Fuploads%2Fallimg%2F110915%2F29-11091512035335.jpg",
        });
      },
      appendSlide: function (slideText) {
        this.slides.push({
          "title": "bajian append",
          "value": "#",
          "image": "https://ss1.bdstatic.com/70cFuXSh_Q1YnxGkpoWK1HF6hhy/it/u=3439730901,2621203596&fm=27&gp=0.jpg",
        });
      },
      removeSlide: function () {

      },
      prev: function () {
        console.log('prev click');
        window.t=this;
        this.$refs.test_prev_next.prev();
      },
      next: function () {
        console.log('next click');
        window.t=this;
        this.$refs.test_prev_next.next();
      }
    }
  }
</script>

<style>
  .title-mask{
    opacity: 0.8;
    background-color: black;
    position: absolute;
    bottom: 0px;
    left: 0px;
    height: 1.25rem;/* 50px */
    line-height: 1.25rem;/* 50px */
    text-overflow: ellipsis;
    width: 100%;
    color: #fff
  }

  .button-prev{
    position: absolute;
    background: transparent;
    left: 15%;
    top: 40%;
    color: red;
    font-size: 40px;
    cursor: pointer;
    z-index: 99;
  }


  .button-next{
    position: absolute;
    background: transparent;
    right: 15%;
    top: 40%;
    color: red;
    font-size: 40px;
    cursor: pointer;
    z-index: 99;
  }


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

  .custom-slide-container-class {
    height: 150px;
    margin-bottom: 80px;
    width: 100%;
    max-width: 350px;
    margin: 0 auto

  }

  /*change the pagination u like */
  .custom-pagination-class.active {
    background: #FB7299 !important;
  }
  .custom-pagination-class {
    background: #fff !important;
    opacity: 1 !important;
    margin: 0 2px !important;
    z-index: 9;
  }

  .custom-pagination-container-class {
    margin-right: 10px !important;
    width: 95% !important;
    text-align: right !important;
  }

</style>
