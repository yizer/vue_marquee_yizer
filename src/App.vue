<template>
  <div id="app">
    <div class="hello">
      <div>

        <Marquee :duration="600" :interval="4000" :showNumber="2" direction="down">
          <li v-for="(item,index) in reverseData" :key="index">{{item}}</li>
        </Marquee>
      </div>
    </div>
    <router-view/>
  </div>
</template>

<script>
import Marquee from "@/components/Marquee";
export default {
  name: "App",
  components: {
    Marquee
  },
  data() {
    return {
      listData: ["消息1", "消息2", "消息3", "消息4", "消息5"],
      reverseData: ["消息6", "消息5", "消息4", "消息3", "消息2", "消息1"]
    };
  },
  mounted() {}
};
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
li {
  width: 100%;

  text-align: center;
}
</style>
