<template>
  <div class="pageview">
    <div>
      <van-nav-bar title="订单详情" fixed>
        <template #left>
          <van-icon name="arrow-left" size="18" color="#000" @click="onClickLeft()" />
        </template>
        <template #right>
          <van-icon name="weapp-nav" size="18" color="#000" />
        </template>
      </van-nav-bar>
    </div>
    <div style="height:46px"></div>
    <div>
      <div class="topview">
        <div class="item">
          <div class="item-left">订单状态</div>
          <div class="item-right">{{mydata.orderStatusEnumString}}</div>
        </div>
        <div class="item">
          <div class="item-left">剩余时间</div>
          <div class="item-right1">15分钟</div>
        </div>
        <div class="item">
          <div class="item-left">订单编号</div>
          <div class="item-right1">{{mydata.orderNo}}</div>
        </div>
        <div class="item">
          <div class="item-left">下单时间</div>
          <div class="item-right1">2018-10-10 14:00:00</div>
        </div>
      </div>
      <div class="middlevie">
        <div class="middleview-title">收货信息</div>
        <div class="middleview-middlebox">
          <div class="yonghuxinxi">
            <div class="name">收货人：{{mydata.consignee}}</div>
            <div class="phone">{{mydata.phone}}</div>
          </div>
          <div class="address">{{mydata.address}}</div>
        </div>
      </div>
      <div class="middlevie">
        <div class="middleview-title">商品信息</div>
        <div class="shoppingbox">
          <div
            class="shoppingview"
            @click="orderdetail()"
            v-for="(item,index) in mydata.detail"
            v-bind:key="index"
          >
            <div class="shoppingview-item">
              <div class="shoppingview-itemleft">
                <img
                  class="shoppingview-itemleftimage"
                  :src="item.imgUrl ? item.imgUrl : imgsrc"
                  alt
                />
              </div>
              <div class="shoppingview-itemright">
                <div class="itembox1-left">
                  <div class="shoppingview-itemright-toptext">{{item.spuName}}</div>
                  <div class="leixingview">
                    <div class="leixing1">{{item.skuName}</div>
                  </div>
                </div>
                <div class="itembox1-right">
                  <div class="shoppingview-itemright-middleview">
                    <div class="shoppingview-itemright-middleview-left">￥{{item.price}}</div>
                    <div class="shoppingview-itemright-middleview-right">x{{item.skuCount}}</div>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
      <div class="middlevie">
        <div class="middleview-title1">付款信息</div>
        <div class="fukuanjine">
          <div class="fukuanjine-left">合计金额:</div>
          <div class="fukuanjine-right">￥{{mydata.actualPrice}}</div>
        </div>
      </div>
    </div>
    <div class="null"></div>
    <div class="middleviewbtn">
      <div class="middleviewbtn-btn1">取消订单</div>
      <div class="middleviewbtn-btn2">立即支付</div>
    </div>
  </div>
</template>

<script>
import Vue from "vue";
import { NavBar, Icon } from "vant";
import Axios from "axios";
import GLOBAL from "@/api/global_variable.js";
import axios from "axios";
import qs from "qs";
axios.defaults.headers["Content-Type"] = "application/json";
axios.defaults.headers["multi-type"] = "H5";
export default {
  name: "Orderdetail",
  components: {
    [NavBar.name]: NavBar,
    [Icon.name]: Icon,
  },
  data() {
    return {
      cutnumber: 1,
      mydata:{},
      imgsrc:"https://img.yzcdn.cn/vant/ipad.jpeg"
    };
  },
  methods: {
    //跳转到地址列表
    goAddresslist: function () {
      this.$router.push({
        name: "Addresslist",
      });
    },
    onClickLeft: function () {
      this.$router.go(-1); // 返回
    },
    onClickRight: function () {
      Toast("按钮");
    },
    loaddata: function () {
      var that = this;
      let data = {
        id: "123123123",
        typeEnum: 1, //<comment>订单列表中typeEnum</comment>
      };
      var api = GLOBAL.baseURL + "/multiapi/z720v_spuOrder";
      function httpPost(url, data = {}) {
        return new Promise((resolve, reject) => {
          axios.post(url, data).then(
            (res) => {
              resolve(res.data);
            },
            (err) => {
              reject(err);
            }
          );
        });
      }
      httpPost(api, data)
        .then((res) => {
          console.log(res);
          Toast(res.msg);
          that.mydata = res.data;
        })
        .catch((err) => {
          console.log(err);
        });
    },
  },
  mounted() {},
};
</script>

<style scoped>
.null {
  height: 85px;

  background: #f8f8f8;
}
.topview-right1 {
  font-size: 18px;
  color: #000000;
  line-height: 40px;
}
.topview-title {
  display: flex;
  justify-content: space-between;
}
.itembox1-left {
  width: 331rpx;
}
.itembox1-right {
  width: 96rpx;
}
.itembox {
  width: 100%;
  background: #ffffff;
  padding: 4px;
  border-radius: 11px;
  box-sizing: border-box;
  margin-bottom: 10px;
  padding: 10px;
  box-sizing: border-box;
}
.pageview {
  background: #f8f8f8;
  height: 100vh;
}
.leixingview {
  display: flex;
}
.middleviewbtn {
  justify-content: end;
  padding: 15px 0px;
  display: -webkit-box;
  left: 0;
  position: fixed;
  bottom: 0;
  width: 100%;
  z-index: 100;
  background: #ffffff;
}
.middleviewbtn-btn1 {
  width: 100px;
  height: 35px;
  line-height: 35px;
  border-radius: 50px;
  background-color: rgba(255, 0, 0, 0);
  color: rgba(136, 136, 136, 1);
  font-size: 14px;
  text-align: center;
  box-shadow: 0px 0px 0px 0px rgba(185, 185, 185, 1);
  font-family: Arial;
  border: 1px solid rgba(178, 178, 178, 1);
  margin: 0px 4px;
}
.middleviewbtn-btn2 {
  width: 100px;
  height: 35px;
  line-height: 35px;
  border-radius: 50px;
  background-color: rgba(132, 68, 4, 1);
  color: rgba(255, 255, 255, 1);
  font-size: 14px;
  text-align: center;
  box-shadow: 0px 0px 0px 0px rgba(185, 185, 185, 1);
  font-family: Arial;
  margin: 0px 4px;
}
.fukuanjine {
  display: flex;
  justify-content: space-between;
}
.fukuanjine-left {
  font-size: 16px;
  text-align: left;
  font-family: PingFangSC-regular;
  font-weight: bold;
  width: 128px;
  font-family: PingFangSC-Regular, PingFang SC;
  font-weight: 400;
  color: #262626;
  line-height: 30px;
}
.fukuanjine-right {
  color: #262626;
  font-size: 20px;
  text-align: left;
  font-family: PingFangSC-regular;
  font-weight: bold;
}
.shoppingbox {
  border: 1px solid #eeeeee;
  background-color: #fafafa;
  padding: 5px 10px;
  box-sizing: border-box;
}
.shoppingview-itemright {
  margin-left: 10px;
}
.shoppingview-itemright-middleview {
  bottom: 0px;
}
.shoppingview-itemright-middleview-right {
  color: rgba(51, 51, 51, 1);
  font-size: 16px;
  text-align: right;
  font-family: PingFangSC-regular;
  font-weight: bold;
}
.shoppingview-itemright-middleview-left {
  color: #844404;
  font-size: 16px;
  text-align: left;
  font-family: PingFangSC-regular;
  font-weight: bold;
}
.shoppingview-itemright-toptext {
  color: rgba(51, 51, 51, 1);
  font-size: 15px;
  text-align: left;
  font-family: PingFangSC-regular;
  font-weight: bold;
}
.shoppingview-itemright-middleview {
  display: flex;
  justify-content: space-between;
}
.shoppingview-itemleft {
  width: 100px;
  height: 100px;
}
.shoppingview-itemleftimage {
  width: 100%;
  height: 100%;
}
.shoppingview {
  padding: 10px 0px;
  border-bottom: 1px solid #f2f2f2;
}
.shoppingview-item {
  display: flex;
  justify-content: space-between;
}
.leixing1 {
  left: 115px;
  top: 499px;
  width: 50px;
  height: 20px;
  line-height: 20px;
  border-radius: 1px;
  background-color: rgba(242, 242, 242, 1);
  color: rgba(136, 136, 136, 1);
  font-size: 12px;
  text-align: center;
  box-shadow: 0px 0px 0px 0px rgba(185, 185, 185, 1);
  font-family: Arial;
  margin: 0px 2px;
}

.address {
  font-size: 15px;
  text-align: left;
  font-family: PingFangSC-Medium, PingFang SC;
  font-weight: 500;
  color: #999999;
}
.name {
  text-align: left;
  font-family: PingFangSC-regular;
  font-size: 15px;
  font-weight: bold;
  color: #262626;
}
.phone {
  color: rgba(51, 51, 51, 1);
  font-size: 15px;
  text-align: right;
  font-family: PingFangSC-regular;
}
.yonghuxinxi {
  display: flex;
  justify-content: space-between;
  padding-bottom: 10px;
}
.middlevie {
  background-color: #ffffff;
  padding: 0px 20px 10px 20px;
  box-sizing: border-box;
}
.middleview-title {
  line-height: 50px;
  font-size: 20px;
  text-align: left;
  font-family: Arial;
  font-weight: bold;
  color: #262626;
}
.middleview-title1 {
  font-size: 20px;
  font-family: PingFangSC-Medium, PingFang SC;
  font-weight: 500;
  color: #999999;
  line-height: 50px;
  text-align: left;
}
.topview {
  background-color: #ffffff;
  padding: 0px 20px;
  box-sizing: border-box;
  margin: 12px 0px;
}
.item {
  display: flex;
  justify-content: space-between;
  line-height: 50px;
  height: 50px;
}
.item-left {
  color: #262626;
  font-size: 16px;
  text-align: left;
  font-family: Arial;

  font-weight: 900;
}
.item-right {
  color: #1c69d4;
  font-size: 16px;
  text-align: right;
  font-weight: bold;
  font-family: Arial;
}
.item-right1 {
  color: #999999;
  font-size: 14px;
  text-align: right;
  font-family: Arial;
}
</style>