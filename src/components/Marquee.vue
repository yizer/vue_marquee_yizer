/**
 * Created by yizer on 2018/07/27
 * Description: 跑马灯 marquee
 * A vue component which is like a advertising board
 * 一个vue的插件/组件，实现了类似广告牌信息滚动的效果
 */

<template>
  <div class="marquee-list" :style="{height: height + 'px'}">
    <ul class="marquee-wrap" ref="container">
      <slot></slot>
    </ul>
  </div>

</template>
<script>
export default {
  data() {
    return {
      height: "100px",
      itemHeight: 0,
      length: 0,
      currentIndex: 0
    };
  },
  props: {
    // 时间间隔
    interval: {
      type: Number,
      default: 3000 //ms
    },
    // 动画时长
    duration: {
      type: Number,
      default: 600 //ms
    },
    // 滚动方向
    direction: {
      validator(val) {
        return val === "up" || val === "down";
      },
      default: "up"
    },
    // 包裹容器高度
    showNumber: {
      type: Number,
      default: 1 //当前容器内显示个数
    }
  },

  mounted() {
    this.fixList();
    this.start();
  },
  methods: {
    /*
    * 根据方向将第一个或最后一个item复制添加到列表最后或前面，保证下一次轮播连贯
    * 将单个item高度设置为广播视窗高度
    */
    fixList() {
      const that = this;
      let cloneNode;
      let firstItem = this.$refs.container.firstElementChild;
      this.length = this.$refs.container.childNodes.length;
      this.itemHeight = firstItem.offsetHeight;
      // 根据item高度设置视窗container高度
      console.log(this.$refs.container.lastElementChild);
      if (this.direction === "up") {
        // 向上则clone第一个item置于列表末端

        this.$refs.container.childNodes.forEach((item, index) => {
          if (index < that.showNumber) {
            cloneNode = that.$refs.container.childNodes[index].cloneNode(true);
            that.$refs.container.appendChild(cloneNode);
          }
        });
      } else {
        // 向下则clone最后一个item置于列表首部
        this.$refs.container.childNodes.forEach((item, index) => {
          if (index < that.showNumber) {
            let nowlenth = that.$refs.container.childNodes.length;
            let firstItem = that.$refs.container.firstElementChild;
            cloneNode = that.$refs.container.childNodes[
              nowlenth - 1 - index
            ].cloneNode(true);

            // cloneNode = this.$refs.container.lastElementChild.cloneNode(true);
            that.$refs.container.insertBefore(cloneNode, firstItem);
          }
        });

        console.log(this.$refs.container.childNodes);
      }
      this.$nextTick(() => {
        // 计算包裹容器的高度: 单个元素高度* 2
        this.height = this.itemHeight * this.showNumber;
      });
    },
    /*
         * 启动轮播
         */
    start() {
      let currenTransitionTime, currenTranslateY;
      this.length = this.$refs.container.childNodes.length;
      // 方向向下，列表初始时跳转到最后item
      if (this.direction === "down") this.quickJump(false);

      setInterval(() => {
        if (this.direction === "up") {
          this.currentIndex += this.showNumber;
        } else {
          this.currentIndex -= this.showNumber;
        }
        console.log(this.currentIndex);
        // 正常轮播transition时间为用户设置duration时间
        currenTransitionTime = "transform " + this.duration + "ms ease-in-out";
        this.setTransition(this.$refs.container, currenTransitionTime);
        // 正常轮播每次currenTranslateY增加item高度
        if (this.direction === "up") {
          currenTranslateY = -this.currentIndex * this.itemHeight + "px";
        } else {
          currenTranslateY = -this.currentIndex * this.itemHeight + "px";
        }
        console.log(currenTranslateY);
        this.setTransform(
          this.$refs.container,
          "translate3d(0," + currenTranslateY + ",0)"
        );
        // 当滑动到首尾边界替补item时，需即刻跳转到正确item位置
        if (this.currentIndex >= this.length) {
          setTimeout(() => {
            this.quickJump(true);
          }, this.duration);
        } else if (this.currentIndex <= -0) {
          setTimeout(() => {
            this.quickJump(false);
          }, this.duration);
        }
      }, this.interval + this.duration);
    },
    /*
         * 设置transition 0ms，再设置translatet位置启动跳转
         * 由于跳转前后展现的内容完全一样，肉眼看不到跳转过程
         */
    quickJump(toFirst) {
      let currenTranslateY,
        currenTransitionTime = "transform 0ms ease-in-out";
      this.setTransition(this.$refs.container, currenTransitionTime);
      if (toFirst) {
        // 跳转到首个item
        this.currentIndex = 0;
        currenTranslateY = "0px";
      } else {
        // this.currentIndex = this.length - 1;
        // currenTranslateY =
        //   -(this.currentIndex + this.showNumber) * this.itemHeight + "px";

        this.currentIndex = this.length - this.showNumber;
        currenTranslateY = -this.currentIndex * this.itemHeight + "px";
      }

      this.setTransform(
        this.$refs.container,
        "translate3d(0," + currenTranslateY + ",0)"
      );
    },
    /*
    * transition添加浏览器前缀
    * transform同
    */
    setTransition(ele, val) {
      ele.style.transition = val;
      ele.style.WebkitTransition = "-webkit-" + val;
      ele.style.MozTransition = "-moz-" + val;
      ele.style.OTransition = "-o-" + val;
    },
    setTransform(ele, val) {
      ele.style.transform = val;
      ele.style.WebkitTransform = val;
      ele.style.MozTransform = val;
      ele.style.OTransform = val;
    }
  }
};
</script>
<style >
.marquee-list {
  width: 100%;
  overflow: hidden;
  transform: translateZ(0);
}
.marquee-wrap {
  padding: 0;
  margin: 0;
  width: 100%;
  height: auto;
  background-color: #999;
}
.marquee-wrap li {
  margin: 0;
}
</style>