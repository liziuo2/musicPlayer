<template>
  <div class="song_wrapper">
    <ul class="song_list">
      <li
        v-for="(item, index) in list"
        :key="index"
        @dblclick="dbc(item, index)"
        :class="{ active: current == index }"
      >
        {{ item.name }} --- {{ item.artists[0].name }}
      </li>
    </ul>
  </div>
</template>

<script>
import axios from "axios";
// 导入公共vue实例
import bus from "../common/bus";

export default {
  data() {
    return {
      list: "",
      current: 0,
      id: 0,
    };
  },
  methods: {
    dbc(item, index) {
      this.current = index;
      this.$parent.index = this.index;
      this.$parent.id = item.id;
      axios({
        // 1.获取音乐url
        url: "https://autumnfish.cn/song/url?id=" + item.id,
      }).then((res) => {
        //成功回调
        this.$parent.url = res.data.data[0].url;

        // 2.获取图片
        axios({
          url: "http://www.dongh5.com:9000/song/detail?ids=" + item.id,
        }).then((res) => {
          //成功回调
          this.$parent.imgSrc = res.data.songs[0].al.picUrl;
        });

        // 3.获取用户评论
        axios({
          url: "http://www.dongh5.com:9000/comment/music?id=" + item.id,
        }).then((res) => {
          //成功回调
          // 参数1:事件的名称,不要和冲突,参数2:要传递的值
          bus.$emit("brother", res.data.comments);
        });
      });
    },
  },
};
</script>

<style>
</style>