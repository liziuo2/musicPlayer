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
      axios({
        // 获取音乐url
        url: "https://autumnfish.cn/song/url",
        method: "get",
        params: { id: item.id },
      }).then((res) => {
        //成功回调
        this.$parent.url = res.data.data[0].url;

        // 获取图片
        axios({
          url: "http://www.dongh5.com:9000/song/detail",
          method: "get",
          params: { ids: item.id },
        }).then((res) => {
          //成功回调
          this.$parent.imgSrc = res.data.songs[0].al.picUrl;
        });
      });
    },
  },
};
</script>

<style>
</style>