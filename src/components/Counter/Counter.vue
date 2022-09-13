<template>
  <div
    class="number-container d-flex justify-content-center align-items-center"
  >
    <!-- 减 1 的按钮 -->
    <button type="button" class="btn btn-light btn-sm" @click="sub">-</button>
    <!-- 购买的数量 -->
    <span class="number-box">{{ num }}</span>
    <!-- 加 1 的按钮 -->
    <button type="button" class="btn btn-light btn-sm" @click="add">+</button>
  </div>
</template>

<script>
import bus from "@/components/EventBus.js";
export default {
  props: {
    // 商品的数量
    num: {
      type: Number,
      default: 1,
    },
    id: {
      type: Number,
      required: "true",
    },
  },
  methods: {
    // 点击+1按钮，将数据发送给App组件，使用Eventbus
    add() {
      // 这里并没有改变num的值，而是将num的值+1以后传给了父组件App，App直接修改了商品的count，所以最终显示在文本的数量就会+1，num是自定义属性，接收的是父组件传给它的值，因此也会发生改变，不直接修改自定义属性的值，由父组件传值来决定子组件的自定义属性如何修改
      const results = { id: this.id, value: this.num + 1 };
      // console.log(results);
      // 使用bus发送数据给App,$emit方法
      bus.$emit("sendMsg", results);
    },
    sub() {
      if (this.num - 1 <= 0) {
        return;
      }
      const results = { id: this.id, value: this.num - 1 };
      // console.log(results);
      // 使用bus发送数据给App,$emit方法
      bus.$emit("sendMsg", results);
    },
  },
};
</script>

<style lang="less" scoped>
.number-box {
  min-width: 30px;
  text-align: center;
  margin: 0 5px;
  font-size: 12px;
}

.btn-sm {
  width: 30px;
}
</style>
