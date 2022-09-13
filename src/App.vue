<template>
  <div class="app-container">
    <Header></Header>
    <!-- 循环渲染每一个商品的信息 -->
    <Goods
      v-for="items in this.list"
      :key="items.id"
      :id="items.id"
      :title="items.goods_name"
      :pic="items.goods_img"
      :pri="items.goods_price"
      :state="items.goods_state"
      :count="items.goods_count"
      @state-change="getNewState"
    ></Goods>
    <!-- 父传子将计算属性传给子,用自定义属性方法 -->
    <!-- 使用自定义事件接收从子传过来的数据 -->
    <Footer
      :fullChecked="fullstate"
      :fullPri="fullPrice"
      :all="total"
      @full-change="selectAll"
    ></Footer>
  </div>
</template>

<script>
// 1.导入需要使用的组件
import Header from "@/components/Header/Header.vue";
import Goods from "@/components/Goods/Goods.vue";
import Footer from "@/components/Footer/Footer.vue";
// 1.导入axios请求库
import axios from "axios";
// 导入eventBus模块
import bus from "@/components/EventBus.js";
export default {
  data() {
    return {
      // 用来存储请求回来的列表数据，默认为空
      list: [],
    };
  },
  // 计算属性，用来计算当前商品是否全部被选中
  computed: {
    // 计算所有的商品是否被全选
    fullstate: function () {
      // 注意计算属性的书写方法
      return this.list.every((items) => items.goods_state === true);
    },
    // 计算所有商品的总价
    fullPrice() {
      return this.list
        .filter((items) => {
          return items.goods_state === true;
        })
        .reduce((amt, items) => {
          return (amt = amt + items.goods_count * items.goods_price);
        }, 0);
    },
    // 已勾选商品的总数量
    total() {
      return this.list
        .filter((items) => {
          return items.goods_state === true;
        })
        .reduce((t, items) => {
          return (t += items.goods_count);
        }, 0);
    },
  },
  created() {
    // 2.在created生命周期函数中调用获取数据的方法
    this.initCartList();
    // 在created生命周期函数中接收子组件Counter通过eventbus传递过来的数据
    // 用回调函数解决this的指向问题
    bus.$on("sendMsg", (val) => {
      // console.log(val);
      this.list.some((items) => {
        if (val.id === items.id) {
          items.goods_count = val.value;
          return true;
        }
      });
    });
  },
  // 3.注册需要使用的组件
  components: {
    Header,
    Goods,
    Footer,
  },
  // 2.声明使用axios请求服务器数据的函数
  methods: {
    async initCartList() {
      // 返回的是promise实例，使用await解析出数据，解构赋值得到data，重命名为res
      const { data: res } = await axios.get("https://www.escook.cn/api/cart");
      // 只要请求回来的数据在页面渲染的时候要用到，就要将数据转存到data当中
      if (res.status === 200) {
        this.list = res.list;
      }
    },
    // 父组件监听state-change自定义事件，并接收参数，将参数保存在list当中
    getNewState(e) {
      console.log(e);
      // 这里要用some方法遍历当前元素中id正确的元素，不能用索引给状态改变，因为添加商品之后索引是没有意义的
      // this.list[e.id - 1].goods_state = e.state; //错误
      this.list.some((items) => {
        if (e.id === items.id) {
          items.goods_state = e.state;
          return true;
        }
      });
    },
    // 父组件监听footer的自定义事件并接受数据是否全选
    selectAll(e) {
      // 循环list中的每一个对象，将其中的goods_state修改为e
      this.list.forEach((items) => {
        items.goods_state = e;
      });
    },
  },
};
</script>

<style lang="less" scoped>
.app-container {
  padding-top: 45px;
  padding-bottom: 50px;
}
</style>
