<template>
  <div class="goods-container">
    <!-- 左侧图片 -->
    <div class="thumb">
      <div class="custom-control custom-checkbox">
        <!-- 复选框 -->
        <input
          type="checkbox"
          class="custom-control-input"
          :id="'cb' + id"
          :checked="state"
          @change="stateChange"
        />
        <label class="custom-control-label" :for="'cb' + id">
          <!-- 商品的缩略图 -->
          <img :src="pic" alt="" />
        </label>
      </div>
    </div>
    <!-- 右侧信息区域 -->
    <div class="goods-info">
      <!-- 商品标题 -->
      <h6 class="goods-title">{{ title }}</h6>
      <div class="goods-info-bottom">
        <!-- 商品价格 -->
        <span class="goods-price">￥{{ pri }}</span>
        <!-- 商品的数量 -->
        <Counter :num="count" :id="id"></Counter>
      </div>
    </div>
  </div>
</template>

<script>
// 导入需要使用的组件
import Counter from "@/components/Counter/Counter.vue";
export default {
  components: {
    Counter,
  },
  props: {
    // 商品id
    id: {
      required: "true",
      type: Number,
    },
    // 商品标题
    title: {
      default: "",
      type: String,
    },
    // 商品缩略图
    pic: {
      default: "",
      type: String,
    },
    // 商品单价
    pri: {
      default: 0,
      type: Number,
    },
    // 商品的勾选状态
    state: {
      default: "ture",
      type: Boolean,
    },
    // 商品的数量
    count: {
      type: Number,
      default: 1,
    },
  },
  methods: {
    stateChange(e) {
      // 1. 监听复选框的change事件，定义并触发state-change事件，将id和state传递给父组件
      this.$emit("state-change", { id: this.id, state: e.target.checked });
    },
  },
};
</script>

<style lang="less" scoped>
.goods-container {
  + .goods-container {
    border-top: 1px solid #efefef;
  }
  padding: 10px;
  display: flex;
  .thumb {
    display: flex;
    align-items: center;
    img {
      width: 100px;
      height: 100px;
      margin: 0 10px;
    }
  }

  .goods-info {
    display: flex;
    flex-direction: column;
    justify-content: space-between;
    flex: 1;
    .goods-title {
      font-weight: bold;
      font-size: 12px;
    }
    .goods-info-bottom {
      display: flex;
      justify-content: space-between;
      .goods-price {
        font-weight: bold;
        color: red;
        font-size: 13px;
      }
    }
  }
}
</style>
