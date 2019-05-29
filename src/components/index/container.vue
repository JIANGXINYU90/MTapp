<template>
  <div class="m-istyle">
    <dl @mouseover="over" :class="nav.class">
      <dt>{{nav.title}}</dt>
      <dd
        v-for="(item, index) in nav.list"
        :key="index"
        :class="{active: kind == item.tab}"
        :data-type="item.tab"
      >{{item.text}}</dd>
    </dl>
    <ul class="ibody">
      <li v-for="(item, index) in resultsData[kind]" :key="index">
        <el-card :body-style="{ padding: '0px' }" shadow="never">
          <img :src="item.image" class="image">
          <div class="cbody">
            <div class="title" :title="item.title">{{item.title}}</div>
            <div class="sub-title" :title="item.sub_title">{{item.sub_title}}</div>
            <div class="price-info" v-if="item.rentNum && item.price_info.current_price">
              <span class="current-price-wrapper">
                <span class="price-symbol numfont">¥</span>
                <span class="current-price numfont">{{item.price_info.current_price}}</span>
              </span>
              <span class="old-price">门市价￥{{item.price_info.old_price}}</span>
              <span class="sold bottom-right-info">{{item.address}}</span>
            </div>
            <div class="price-info" v-else-if="!item.rentNum">
              <span class="current-price-wrapper">
                <span class="price-symbol numfont">¥</span>
                <span class="current-price numfont">抢光了</span>
              </span>
            </div>
            <div class="price-info" v-else>
              <span class="current-price-wrapper">
                <span class="price-symbol numfont">¥</span>
                <span class="current-price numfont">{{item.price_info.avg_price}}</span>
                <span class="units">/{{item.price_info.units}}均</span>
              </span>
            </div>
          </div>
        </el-card>
      </li>
      <li></li>
      <li></li>
      <li></li>
      <li></li>
      <li></li>
    </ul>
  </div>
</template>

<script>
import api from "@/api/index.js"
export default {
  data() {
    return {
      kind: "all",
      resultsData: {},
      list: [
        {
          image:
            "//p1.meituan.net/msmerchant/a316aecb8e8db3a2dda8e7d7f93137a41454100.jpg@736w_416h_1e_1c",
          title: "必胜客（创意园店）",
          sub_title: "【随新享比萨2人餐A008】1份",
          price_info: {
            current_price: null,
            old_price: null,
            avg_price: 18,
            units: '人'
          },
          rentNum: 10,
          address: "中联广场"
        }, {
          image:
            "//p1.meituan.net/msmerchant/a316aecb8e8db3a2dda8e7d7f93137a41454100.jpg@736w_416h_1e_1c",
          title: "必胜客（创意园店）",
          sub_title: "【随新享比萨2人餐A008】1份",
          price_info: {
            current_price: 18,
            old_price: 36,
            avg_price: null,
            units: null
          },
          rentNum: 10,
          address: "中联广场"
        }
      ]
    };
  },
  created() {
    api.resultsByKeywords().then( res => {
      this.resultsData = res.data.data;
    })
  },
  props: ["nav"],
  methods: {
    over(e) {
      let dom = e.target;
      let tagName = dom.tagName.toLowerCase();
      if (tagName != "dd") {
        return false;
      }
      this.kind = dom.getAttribute("data-type");
      // 动态获取数据ajax请求
    }
  }
};
</script>

<style lang="scss">
@import "@/assets/css/index/artistic.scss";
</style>