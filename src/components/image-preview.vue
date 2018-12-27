<template>
  <div class="imagePreview" v-if="urls.length" @click="closePreview">
    <img
      class="image"
      ref="img"
      :src="getImgUrl"
      alt=""
      :style="style"
      draggable="false"
      @click.stop
      @error="imgError"
      @mousedown="mousedown"
      @mousewheel="mousewheel($event)"
      @DOMMouseScroll="mousewheel($event)">
    <i class="iconfont icon-fanye prev" v-show="index > 0" @click.stop="index--"></i>
    <i class="iconfont icon-PathCopy next" v-show="index < urls.length - 1" @click.stop="index++"></i>
    <div class="footer">
      <i class="iconfont icon-search-plus" @click.stop="setScale(true)"></i>
      <i class="iconfont icon-search-minus" @click.stop="setScale(false)"></i>
      <i class="iconfont icon-undo-alt" @click.stop="setRotate(true)"></i>
      <i class="iconfont icon-redo-alt" @click.stop="setRotate(false)"></i>
    </div>
  </div>
</template>

<script type="text/ecmascript-6">
  import '../common/iconfont/iconfont.css';

  const SCALE = 0.2;
  const ROTATE = 90;

  export default {
    props: {
      urls: {
        type: Array,
        default() {
          return [];
        },
      },
      current: {
        type: String,
        default: '',
      },
    },
    data() {
      return {
        index: 0,
        style: {
          position: 'relative',
          left: 0,
          top: 0,
          transform: '',
        },
        scale: 1,
        rotate: 0,
        left: 0,
        top: 0,
      };
    },
    created() {
      if (this.current) {
        this.index = this.urls.findIndex((url) => {
          return url === this.current;
        });
      }

      this.initDocumentEvent();
    },
    computed: {
      getImgUrl() {
        return this.urls[this.index];
      },
    },
    watch: {
      // 左右切换时重置样式
      index() {
        this.resetStyle();
      },
      urls() {
        this.initDocumentEvent();
      }
    },
    methods: {
      initDocumentEvent() {
        if (this.urls.length) {
          // document 添加事件
          document.onmousemove = this.mousemove;
          document.onmouseup = this.mouseup;
          document.body.onselectstart = document.body.ondrag = () => {
            return false;
          };
        } else {
          // 移除 document 事件
          document.onmousemove =
              document.onmouseup =
                  document.body.onselectstart =
                      document.body.ondrag = null;
        }
      },
      // 重置图片样式
      resetStyle() {
        this.style = {
          position: 'relative',
          left: 0,
          top: 0,
          transform: '',
        };
        this.scale = 1;
        this.rotate = 0;
        this.left = 0;
        this.top = 0;
      },
      // 设置样式
      setTransform() {
        if (this.scale <= SCALE) {
          this.scale = SCALE;
        }
        this.style.transform = `scale(${this.scale}) rotate(${this.rotate}deg)`;
      },
      // 图片加载失败
      imgError() {
        // console.log('图片加载失败');
      },
      // 关闭预览
      closePreview() {
        this.urls.splice(0);
        this.resetStyle();
      },
      // 滚轮滚动事件
      mousewheel(e) {
        // const delta = (e.originalEvent.wheelDelta && (e.originalEvent.wheelDelta > 0 ? 1 : -1)) ||  // chrome & ie
        //   (e.originalEvent.detail && (e.originalEvent.detail > 0 ? -1 : 1));              // firefox
        //
        // console.log(delta);

        let direct = 0;
        e = e || window.event;
        if (e.wheelDelta) {  // 判断浏览器IE，谷歌滑轮事件
          if (e.wheelDelta > 0) {  // 当滑轮向上滚动时
            direct = 1;
          }
          if (e.wheelDelta < 0) {  // 当滑轮向下滚动时
            direct = -1;
          }
        } else if (e.detail) {  // Firefox滑轮事件
          if (e.detail < 0) {  // 当滑轮向上滚动时
            direct = 1;
          }
          if (e.detail > 0) {  // 当滑轮向下滚动时
            direct = -1;
          }
        }

        this.setScale(direct > 0);
      },
      // 鼠标按下
      mousedown(e) {
        const left = this.$refs.img.offsetLeft;
        const top = this.$refs.img.offsetTop;
        this.oldX = e.clientX;
        this.oldY = e.clientY;
        this.left = left;
        this.top = top;
        this.canMove = true;
        this.style = {
          ...this.style,
          position: 'absolute',
          left: `${left}px`,
          top: `${top}px`,
        };
      },
      // 鼠标移动
      mousemove(e) {
        if (!this.canMove) {
          return;
        }
        const moveX = e.clientX - this.oldX;
        const moveY = e.clientY - this.oldY;
        this.style.left = `${this.left + moveX}px`;
        this.style.top = `${this.top + moveY}px`;
      },
      // 鼠标抬起
      mouseup() {
        this.canMove = false;
      },
      // 缩放
      setScale(add) {
        if (add) {
          this.scale += SCALE;
        } else {
          this.scale -= SCALE;
        }
        this.setTransform();
      },
      // 旋转
      setRotate(left) {
        if (left) {
          this.rotate -= ROTATE;
        } else {
          this.rotate += ROTATE;
        }
        this.setTransform();
      }
    }
  };
</script>

<style scoped>
  .imagePreview {
    position: fixed;
    top: 0;
    left: 0;
    z-index: 9999;
    display: flex;
    justify-content: center;
    align-items: center;
    width: 100%;
    height: 100%;
    background: rgba(0, 0, 0, 0.5);
  }

  .image {
    max-width: 100%;
    max-height: 100%;
    -webkit-transition: -webkit-transform 0.3s;
    transition: -webkit-transform 0.3s;
    transition: transform 0.3s;
    transition: transform 0.3s, -webkit-transform 0.3s;
    -webkit-user-select: none;
    -moz-user-select: none;
    -ms-user-select: none;
    user-select: none;
  }

  .prev, .next {
    position: absolute;
    top: 50%;
    padding: 10px;
    border-radius: 50%;
    background: #000;
    -webkit-transform: translateY(-50%);
    transform: translateY(-50%);
    font-size: 20px;
    color: #fff;
    cursor: pointer;
    z-index: 3;
    line-height: 22px;
  }

  .prev:hover, .next:hover {
    background: lightgreen;
  }

  .prev {
    left: 80px;
  }

  .next {
    right: 80px;
  }

  .footer {
    position: absolute;
    left: 0;
    bottom: 0;
    z-index: 3;
    width: 100%;
    padding-bottom: 20px;
    text-align: center;
  }

  .footer .iconfont {
    margin: 0 10px;
    font-size: 26px;
    color: #fff;
    cursor: pointer;
  }
</style>