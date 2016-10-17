<template>
    <div class="swiper"
    :class="[direction, {'dragging': dragging}]"
    @touchstart="_onTouchStart"
    @wheel="_onWheel">
    <div class="swiper-wrap"
    ref="swiper-wrap"
    :style="{'transform' : 'translate3d(' + translateX + 'px,' + translateY + 'px, 0)'}"
    @transitionend="_onTransitionEnd">
    <slot></slot>
</div>
<div class="swiper-pagination"
v-show="paginationVisible">
<span class="swiper-pagination-bullet"
:class="{'active': index+1===currentPage}"
v-for="(slide,index) in slides"
:key="index"
@click="setPage(index+1)"></span>
</div>
</div>
</template>
<script type="text/babel">
    const VERTICAL = 'vertical';
    const HORIZONTAL = 'horizontal';

    export default {
        props: {
            direction: {
                type: String,
                default: HORIZONTAL,
                validator: (value) => [VERTICAL, HORIZONTAL].indexOf(value) > -1
            },
            mousewheelControl: {
                type: Boolean,
                default: true
            },
            performanceMode: {
                type: Boolean,
                default: false
            },
            paginationVisible: {
                type: Boolean,
                default: false
            },
            repeating: {
                type: Boolean,
                default: false
            },
            slides: {
                type: Array,
                default: []
            },
            auto: {
                type: Number,
                default: 0 //0 to not auto
            }
        },
        data() {
            return {
                currentPage: 1,
                intervalId: 0,
                lastPage: 1,
                translateX: 0,
                translateY: 0,
                startTranslateX: 0,
                startTranslateY: 0,
                delta: 0,
                dragging: false,
                startPos: null,
                transitioning: false,
                slideEls:[]
            };
        },
        watch:{
            slides:function(){
                this.slideEls = this.$refs['swiper-wrap'].children;
            }
        },
        mounted(){
            this.$nextTick(function () {
                this._onTouchMove = this._onTouchMove.bind(this);
                this._onTouchEnd = this._onTouchEnd.bind(this);
                this.slideEls = this.$refs['swiper-wrap'].children;
                this.autoBegin();
            })
        },
        methods: {

            next() {
                var page = this.currentPage;
                if (page < this.slideEls.length) {
                    page++;
                    this.setPage(page);
                } else {
                    if (this.repeating) this.setPage(1);
                    else this._revert();
                }
            },
            autoBegin() {
                if (!this.auto) return;
                this.intervalId=setInterval(()=>{
                    this.next();
                }, this.auto);
            },
            autoStop() {
                clearInterval(this.intervalId)
            },
            prev() {
                var page = this.currentPage;
                if (page > 1) {
                    page--;
                    this.setPage(page);
                } else {
                    if (this.repeating) this.setPage(this.slideEls.length);
                    else this._revert();
                }
            },
            setPage(page) {
                this.autoStop();
                var propName, translateName;
                this.lastPage = this.currentPage;
                this.currentPage = page;

                if (this.isHorizontal()) {
                    propName = 'clientWidth';
                    translateName = 'translateX';
                } else {
                    propName = 'clientHeight';
                    translateName = 'translateY';
                }
                //偏移的大小
                this[translateName] = -[].reduce.call(this.slideEls, function (total, el, i) {
                //previousValue,currentValue,currentIndex
                return i > page - 2 ? total : total + el[propName];
            }, 0);
                this._onTransitionStart();
                this.autoBegin();
            },
            isHorizontal() {
                return this.direction === HORIZONTAL;
            },
            isVertical() {
                return this.direction === VERTICAL;
            },
            _onTouchStart(e) {
                this.autoStop()
                this.startPos = this._getTouchPos(e);
                this.delta = 0;
                this.startTranslateX = this.translateX;
                this.startTranslateY = this.translateY;
                this.startTime = new Date().getTime();
                this.dragging = true;
                e.stopPropagation();
                document.addEventListener('touchmove', this._onTouchMove, false);
                document.addEventListener('touchend', this._onTouchEnd, false);
                document.addEventListener('mousemove', this._onTouchMove, false);
                document.addEventListener('mouseup', this._onTouchEnd, false);
            },
            _onTouchMove(e) {
                this.delta = this._getTouchPos(e) - this.startPos;
                if (!this.performanceMode) {
                    if (this.isHorizontal()) {
                        this.translateX = this.startTranslateX + this.delta;
                        this.$emit('slider-move', this.translateX);
                    } else {
                        this.translateY = this.startTranslateY + this.delta;
                        this.$emit('slider-move', this.translateY);
                    }
                }

                if (this.isVertical() || this.isHorizontal() && Math.abs(this.delta) > 0) {
                    e.preventDefault();
                    e.stopPropagation();
                }
            },
            _onTouchEnd(e) {
                this.autoBegin()
                this.dragging = false;
                var isQuickAction = new Date().getTime() - this.startTime < 1000;
                if (this.delta < -100 || (isQuickAction && this.delta < -15)) {
                    this.next();
                } else if (this.delta > 100 || (isQuickAction && this.delta > 15)) {
                    this.prev();
                } else {
                    this._revert();
                }

                document.removeEventListener('touchmove', this._onTouchMove);
                document.removeEventListener('touchend', this._onTouchEnd);
                document.removeEventListener('mousemove', this._onTouchMove);
                document.removeEventListener('mouseup', this._onTouchEnd);
            },
            _onWheel(e) {
                if (this.mousewheelControl) {
                    // TODO Support apple magic mouse and trackpad.
                    if (!this.transitioning) {
                        if (e.deltaY > 0) {
                            this.next();
                        } else {
                            this.prev();
                        }
                    }

                    if (this._isPageChanged()) e.preventDefault();

                }
            },
            _revert() {
                this.setPage(this.currentPage);
            },
            _getTouchPos(e) {
                var key = this.isHorizontal() ? 'pageX' : 'pageY';
                return e.changedTouches ? e.changedTouches[0][key] : e[key];
            },
            _onTransitionStart() {
                this.transitioning = true;
                if (this._isPageChanged()) {
                    this.$emit('slide-change-start', this.currentPage);
                } else {
                    this.$emit('slide-revert-start', this.currentPage);
                }
            },
            _onTransitionEnd(e) {
                this.transitioning = false;
                if (this._isPageChanged()) {
                    this.$emit('slide-change-end', this.currentPage);
                } else {
                    this.$emit('slide-revert-end', this.currentPage);
                }
                e.stopPropagation()

            },
            _isPageChanged() {
                return this.lastPage !== this.currentPage;
            }
        }
    };
</script>


<style lang="less">
    .swiper {
      position: relative;
      overflow: hidden;

      .swiper-wrap {
        display: flex;
        display: inline-flex;
        width: 100%;
        height: 100%;
        transition: all 0.4s ease;

        > div {
          overflow: hidden;
          flex-shrink: 0;
          width: 100%;
          height: 100%;
      }
  }

  &.horizontal .swiper-wrap {
    flex-direction: row;
}

&.vertical .swiper-wrap {
    flex-direction: column;
}

&.dragging .swiper-wrap {
    transition: none;
}

.swiper-pagination {
    position: absolute;

    .swiper-pagination-bullet {
      width: 8px;
      height: 8px;
      border-radius: 50%;
      background-color: #000000;
      opacity: .2;
      cursor:pointer
  }

  .swiper-pagination-bullet.active {
      background: #007aff;
      opacity: 1;
  }
}

&.vertical .swiper-pagination {
    right: 10px;
    top: 50%;
    transform: translate3d(0, -50%, 0);

    .swiper-pagination-bullet {
      display: block;
      margin: 3px 0;
  }
}

&.horizontal .swiper-pagination {
    bottom: 10px;
    width: 100%;
    text-align: center;

    .swiper-pagination-bullet {
      display: inline-block;
      margin: 0 3px;
  }
}
}

</style>
