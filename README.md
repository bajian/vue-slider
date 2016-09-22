# slider 
auto banner for more function
[online-demo](https://cdn.rawgit.com/bajian/vue-slider/master/dist/demo.html)

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
    <slider id="swiper_horizontal"
    :pagination-visible="true"
    :slides="slides"
    :repeating="true"
    :auto="5000">
    <a v-for="slide in slides" track-by="$index" :href="slide.value">
      <img width="500" height="156" :src="slide.image" />
    </a>
  </slider>
```

## Api
### Properties
| Name                 | Type      | Default      | Description                                                        |
|----------------------|-----------|--------------|--------------------------------------------------------------------|
| direction            | `String`  | `"horizontal"` | Could be 'horizontal' or 'vertical' (for vertical slider).         |
| mousewheel-control   | `Boolean` | `true`       | Set to true to enable navigation through slides using mouse wheel. |
| pagination-visible   | `Boolean` | `false`      | Toggle (hide/true) pagination container visibility when click on Slider's container    |
| performace-mode      | `Boolean` | `false`      | Disable advance effect for better performance.      
| slides      | `Array` | `[]`      | the banner data be used to observe by pagination when you add or remove a child slide  
| repeating      | `Boolean` | `false`      | Set to true to enable repeating from last to first or first to last                 |
| auto      | `Number` | `0`      | Set to 0ms to disable silders auto change     |
| ==================== | ========= | ============ | =================== |

### Methods
| Method            | Description              |
|-------------------|--------------------------|
| next()            | Go next page.            |
| prev()            | Go previous page.        |
| setPage(`Number`) | Set current page number. |

### Events
| Name                            | Parameters | Description                                                                                                                                                  |
|--------------------|------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------|
| slide-change-start | `pageNumber`     | Fire in the beginning of animation to other slide (next or previous).                                                                                        |
| slide-change-end   | `pageNumber`     | Will be fired after animation to other slide (next or previous).                                                                                             |
| slide-revert-start | `pageNumber`     | Fire in the beginning of animation to revert slide (no change).                                                                                              |
| slide-revert-end   | `pageNumber`     | Will be fired after animation to revert slide (no change).                                                                                                   |
| slider-move        | `offset`         | Callback function, will be executed when user touch and move finger over Swiper and move it. Receives swiper instance and 'touchmove' event as an arguments. |
| ================== | ================ | ============================ |

###base on
[vue-swiper](https://github.com/weilao/vue-swiper)

For detailed explanation on how things work, checkout the [guide](http://vuejs-templates.github.io/webpack/) and [docs for vue-loader](http://vuejs.github.io/vue-loader).
