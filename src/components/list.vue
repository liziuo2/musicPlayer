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
    };
  },
  methods: {
    dbc(item, index) {
      this.current = index;
      axios({
        url: "https://autumnfish.cn/song/url",
        method: "get",
        params: { id: item.id },
      }).then((res) => {
        //成功回调
        this.$parent.src = res.data.data[0].url;
      });
    },
  },
};
</script>

<style>
</style>