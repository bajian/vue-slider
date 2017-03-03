# slider 
auto banner for more function for Vue2.x

##demo

[live-demo](https://cdn.rawgit.com/bajian/vue-slider/master/dist/demo4.html)

> A Vue.js project

## Build Setup

``` bash
# install dependencies
npm install

# serve with hot reload at localhost:8080
npm run dev

# build for production with minification
npm run build

```

```html
    <slider
    :pagination-visible="true"
    :slides="slides"
    :repeating="true"
    :auto="5000">

    <div v-for="(slide,index) in slides" :key="index">
      <a :href="slide.value">
        <img width="350" height="150" :src="slide.image" />
      </a>
    </div>
  </slider>
```

## Api
### Properties
| Name                 | Type      | Default      | Description                                                        |
|----------------------|-----------|--------------|------------------------------------------------------------------|
| mousewheel-control   | `Boolean` | `true`       | Set true to enable navigation through slides using mouse wheel. |
| pagination-visible   | `Boolean` | `false`      | Toggle (hide/true) pagination container visibility when click on Slider's container    |
| performace-mode      | `Boolean` | `false`      | Disable animation for better performance for bad android.      
| slides      | `Array` | `[]`      | the banner data just be used to observe by pagination when you add or remove a child slide  
| repeating      | `Boolean` | `false`      | Set true to enable repeating from last to first or first to last                 |
| auto      | `Number` | `0`      | Set to 0ms to disable silders auto change     |
| ==================== | ========= | ============ | =================== |

### Events
| Name                            | Parameters | Description                                                                                                                                                  |
|--------------------|------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------|
| slide-change-start | `pageNumber`     | Fire in the beginning of animation to other slide (next or previous).                                                                                        |
| slide-change-end   | `pageNumber`     | Will be fired after animation to other slide (next or previous).                                                                                             |
| slide-revert-start | `pageNumber`     | Fire in the beginning of animation to revert slide (no change).                                                                                              |
| slide-revert-end   | `pageNumber`     | Will be fired after animation to revert slide (no change).                                                                                                   |
| slider-move        | `offset`         | Callback function, will be executed when user touch and move finger over Swiper and move it. Receives swiper instance and 'touchmove' event as an arguments. |
| ================== | ================ | ============================ |

### Methods
| Name                            | Parameters | Description                                                                                                                                                  |
|--------------------|------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------|
| next() | no     | call it to slide to the next slider eg:this.$refs.test_prev_next.next();                                                                                        |
| prev()   | no     | call it to slide to the previous slider eg:this.$refs.test_prev_next.prev();                                                                                            |
| ================== | ================ | ============================ |

