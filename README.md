# vue-marquee-roll-up

vue 插件：向上轮播的跑马灯/向上走的滚动公告。vue's plugin: a marquee which roll up. mobile-friendly

> A vue plugin/component which is like a advertising board or marquee roll up。 一个 vue 的插件/组件，实现了类似广告牌向上滚动的效果滚动，或者说是向上滚动的跑马灯效果。

### Usage

```html
<template>
  <div>
     <Marquee
     :duration="600"
     :interval="4000"
     :showNumber="2"
     direction="down">
    <li v-for="(item,index) in reverseData" :key="index">{{item}}</li>
        </Marquee>
  </div>
</template>
### Props 参数
      * direction     [String] "down"/"up" 跑马灯的滚动方向,默认"up"
      * showNumber   [Number] 跑马灯容器内显示的个数,每次滚动个数与此相同
      * interval:     (ms) [Number] 轮播间隔时间 the autoplay time。 default: 3000
      * duration:        (ms) [Number]  轮播动画的速度 the speed of animation。 default: 500
```
