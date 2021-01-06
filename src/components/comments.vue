<template>
  <div class="comment_list">
    <div>
      <dl v-for="(item, index) in list" :key="index">
        <dt>
          <img :src="item.user.avatarUrl" alt="" />
        </dt>
        <dd class="name">{{ item.user.nickname }}</dd>
        <dd class="detail">
          {{ item.content }}
        </dd>
      </dl>
    </div>
  </div>
</template>

<script>
import bus from "../common/bus";
export default {
  data() {
    return {
      list: [],
    };
  },
  mounted() {
    // $on在vue中就是提供监听事件的方法
    // 参数1:事件名 参数2:函数
    bus.$on("brother", (val) => {
      this.list = val;
    });
  },
};
</script>

<style>
</style>