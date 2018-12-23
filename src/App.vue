<template>
  <div>
    <router-view></router-view>
  </div>
</template>
<style>
/* #cropper>div:nth-child(3){
  z-index: 5;
} */

#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  overflow: hidden;
}
body {
  padding: 0px 0px;
  margin: 0;
  background-color: #eee;
  overflow: hidden;
}
#cropper > div:nth-child(2) {
  z-index: 5;
  background: transparent;
}
.cropper-view-box {
  outline: 1px solid #eee !important;
}
</style>
<script>
export default {
  created() {
    let _this = this;
    var ua = window.navigator.userAgent.toLowerCase();
    //通过正则表达式匹配ua中是否含有MicroMessenger字符串
    if (ua.match(/MicroMessenger/i) == "micromessenger") {
      this.$axios({
        method: "get",
        url:
          "https://shimmer.neusoft.edu.cn/wechat/web/api/init?url=" +
          encodeURIComponent(location.href.split("#")[0])
      }).then(function(res) {
        wx.config({
          debug: false,
          appId: res.data.appid, // 必填，公众号的唯一标识
          timestamp: res.data.timestamp, // 必填，生成签名的时间戳
          nonceStr: res.data.noncestr, // 必填，生成签名的随机串
          signature: res.data.signature, // 必填，签名，见附录1
          jsApiList: [
            "updateAppMessageShareData",
            "updateTimelineShareData",
            "scanQRCode",
            "chooseImage",
            "uploadImage"
          ] // 必填，需要使用的JS接口列表，所有JS接口列表见附录2
        });
        wx.ready(function() {
          //需在用户可能点击分享按钮前就先调用
          wx.updateAppMessageShareData({
            title: "2019大连东软信息学院新年贺卡", // 分享标题
            desc: "2019大连东软信息学院新年贺卡", // 分享描述
            link: "https://shimmer.neusoft.edu.cn/static/card/#/before", // 分享链接，该链接域名或路径必须与当前页面对应的公众号JS安全域名一致
            imgUrl: "", // 分享图标
            success: function() {}
          });
          wx.updateTimelineShareData({
            title: "2019大连东软信息学院新年贺卡", // 分享标题
            desc: "2019大连东软信息学院新年贺卡", // 分享描述
            link: "https://shimmer.neusoft.edu.cn/static/card/#/before", // 分享链接，该链接域名或路径必须与当前页面对应的公众号JS安全域名一致
            imgUrl: "", // 分享图标
            success: function() {
              // 设置成功
            }
          });
        });
      });
    }
  }
};
</script>
