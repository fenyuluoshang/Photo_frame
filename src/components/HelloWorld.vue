<template>
  <el-container>
    <el-main>
      <div id="reust" :class="{hold:ishold}">
        <img :src="zbase64[0]" :style="{width: lastwidth+'px',height: lastheight+'px',}" id="zpy">
        <img src="./imaaa.png" id="resut">
      </div>
      <div id="bertishi" :class="{hold:tishi}">
        <div class="tishi" @click="yincang"></div>
      </div>
      <div id="sss" ref="container" :style="{width: lastwidth+'px',height: lastheight+'px'}">
        <vueCropper
          ref="cropper"
          :img="option.img"
          :full="option.full"
          :outputSize="option.size"
          :outputType="option.outputType"
          :autoCrop="true"
          :autoCropWidth="maxWidth"
          :autoCropHeight="maxHeight"
          :high="true"
          :fixedBox="true"
          :fixedNumber="fixedNumber"
          :original="false"
          id="cropper"
          :info="false"
          @realTime="realTime"
        ></vueCropper>
        <img :src="option.bg" :style="{width: lastwidth+'px',height: lastheight+'px',}" id="imgbg">
      </div>
    </el-main>

    <el-footer>
      <el-row :gutter="20" id="imgbarr">
        <el-col :span="6">
          <div>
            <div class="fugai" @click="changBG(1)">&nbsp;</div>
            <img src="./2018/1.png" class="showbar">
          </div>
        </el-col>
        <el-col :span="6">
          <div>
            <div class="fugai" @click="changBG(2)">&nbsp;</div>
            <img src="./2018/2.png" class="showbar">
          </div>
        </el-col>
        <el-col :span="6">
          <div>
            <div class="fugai" @click="changBG(3)">&nbsp;</div>
            <img src="./2018/3.png" class="showbar">
          </div>
        </el-col>
        <el-col :span="6">
          <div>
            <div class="fugai" @click="changBG(4)">&nbsp;</div>
            <img src="./2018/4.png" class="showbar">
          </div>
        </el-col>
      </el-row>

      <el-row type="flex" class="row-bg" justify="space-around">
        <el-col :style="{display:iswechat?'none':''}" :span="12">
          <label class="el-button" for="uploads">选择图片</label>
          <input
            type="file"
            id="uploads"
            style="position:absolute; clip:rect(0 0 0 0);"
            accept="image/*"
            @change="uploadImg"
            ref="uploadbtn"
          >
        </el-col>
        <el-col :style="{display:!iswechat?'none':''}" :span="12">
          <el-button @click="uploadImg(null,0)">选择图片</el-button>
        </el-col>
        <el-col :span="12">
          <el-button @click="finish()" v-loading.fullscreen.lock="fullscreenLoading" >生成贺卡</el-button>
        </el-col>
      </el-row>
    </el-footer>
  </el-container>
</template>
<script>
import vueCropper from "vue-cropper";
import { setTimeout } from 'timers';
export default {
  components: {
    vueCropper
  },
  data: function () {
    return {
      fullscreenLoading:false,
      iswechat: false,
      wxready: 0,
      type: 1,
      ishold: true,
      c: [],
      zbase64: [],
      previews: {},
      lastwidth: 100,
      lastheight: 200,
      fullwidth: 100,
      maxWidth: 30000,
      maxHeight: 30000,
      fixedNumber: [6, 9],
      tishi: false,
      msg: "全局作用域变量",
      option: {
        size: 1,
        outputType: "jpg",
        img: "",
        bg: require("./2018/1.png"),
        full: true
      }
    };
  },
  methods: {
    yincang() {
      this.tishi = true;
      console.log("ssss");
      // if (this.wxready === 1)
      // this.uploadImg(none)
      // this.$refs.uploadbtn.click();
    },
    changeImg() {
      this.option.img = this.lists[~~(Math.random() * this.lists.length)].img;
    },
    startCrop() {
      // start
      this.crap = true;
      this.$refs.cropper.startCrop();
    },
    stopCrop() {
      //  stop
      this.crap = false;
      this.$refs.cropper.stopCrop();
    },
    clearCrop() {
      // clear
      this.$refs.cropper.clearCrop();
    },
    // 实时预览函数
    realTime(data) {
      this.previews = data;
    },
    changBG(type) {
      this.type = type;
      console.log(type);
      switch (type) {
        case 1:
          this.option.bg = require("./2018/1.png");
          break;
        case 2:
          this.option.bg = require("./2018/2.png");
          break;
        case 3:
          this.option.bg = require("./2018/3.png");
          break;
        case 4:
          this.option.bg = require("./2018/4.png");
          break;
      }
      console.log(this.type);
    },
    finish() {
      // 输出
      this.fullscreenLoading = true;

      this.ishold = false;
      var sdsssd = [];
      this.$refs.cropper.getCropBlob(data => {
        var c = document.createElement("canvas");
        var ctx = c.getContext("2d");
        c.width = 600;
        c.height = 900;
        ctx.rect(0, 0, c.width, c.height);
        console.log("创建canvas");
        ctx.fillStyle = "#fff";
        ctx.fill();
        console.log("进入绘制");
        var img = new Image();
        img.src = window.URL.createObjectURL(data);
        var imgsrc = this.option.bg;
        var type = this.type;
        switch (type) {
          case 1:
            img.onload = function () {
              console.log("绘制内容");
              ctx.drawImage(img, 0, 0, c.width, c.height);
              var img2 = new Image();
              img2.src = require("./2018/1.png");
              img2.onload = function () {
                console.log("绘制背景");
                ctx.drawImage(img2, 0, 0, c.width, c.height);
                console.log("填充数组");
                sdsssd.push(c.toDataURL("image/png", 0.8));
                console.log(sdsssd[0]);
                document.getElementById("zpy").src = sdsssd[0];
              };
            };
            break;
          case 2:
            img.onload = function () {
              console.log("绘制内容");
              ctx.drawImage(img, 0, 0, c.width, c.height);
              var img2 = new Image();
              img2.src = require("./2018/2.png");
              img2.onload = function () {
                console.log("绘制背景");
                ctx.drawImage(img2, 0, 0, c.width, c.height);
                console.log("填充数组");
                sdsssd.push(c.toDataURL("image/png", 0.8));
                console.log(sdsssd[0]);
                document.getElementById("zpy").src = sdsssd[0];
              };
            };
            break;
          case 3:
            img.onload = function () {
              console.log("绘制内容");
              ctx.drawImage(img, 0, 0, c.width, c.height);
              var img2 = new Image();
              img2.src = require("./2018/3.png");
              img2.onload = function () {
                console.log("绘制背景");
                ctx.drawImage(img2, 0, 0, c.width, c.height);
                console.log("填充数组");
                sdsssd.push(c.toDataURL("image/png", 0.8));
                console.log(sdsssd[0]);
                document.getElementById("zpy").src = sdsssd[0];
              };
            };
            break;
          case 4:
            img.onload = function () {
              console.log("绘制内容");
              ctx.drawImage(img, 0, 0, c.width, c.height);
              var img2 = new Image();
              img2.src = require("./2018/4.png");
              img2.onload = function () {
                console.log("绘制背景");
                ctx.drawImage(img2, 0, 0, c.width, c.height);
                console.log("填充数组");
                sdsssd.push(c.toDataURL("image/png", 0.8));
                console.log(sdsssd[0]);
                document.getElementById("zpy").src = sdsssd[0];
                this.fullscreenLoading = false;
              };
            };
            break;
        }
        this.fullscreenLoading = false;
      });
    },

    down(type) {
      // event.preventDefault()
      var aLink = document.createElement("a");
      aLink.download = "demo";
      // 输出
      if (type === "blob") {
        this.$refs.cropper.getCropBlob(data => {
          this.downImg = data;
          aLink.href = data;
          aLink.click();
        });
      } else {
        this.$refs.cropper.getCropData(data => {
          this.downImg = data;
          aLink.href = data;
          aLink.click();
        });
      }
    },
    uploadImg(e, num) {
      //上传图片
      if (!this.iswechat) {
        //非微信处理区
        var file = e.target.files[0];
        console.log(file);
        if (!/\.(gif|jpg|jpeg|png|bmp|JPG|PNG)$/.test(e.target.value)) {
          alert("图片类型必须是.gif,jpeg,jpg,png,bmp中的一种");
          return false;
        }
        var reader = new FileReader();
        reader.onload = e => {
          let data;
          if (typeof e.target.result === "object") {
            // 把Array Buffer转化为blob 如果是base64不需要
            data = window.URL.createObjectURL(new Blob([e.target.result]));
          } else {
            data = e.target.result;
          }
          num = 1;
          // console.log(num)
          if (num === 1) {
            this.option.img = data;
            console.log(data);
          }
        };
        reader.readAsArrayBuffer(file);
      }
      else {
        //微信处理区
        let _this = this;
        // if (this.wxready == 1)
        wx.chooseImage({
          count: 1, // 默认9
          sizeType: ['compressed'], // 可以指定是原图还是压缩图，默认二者都有
          sourceType: ['album', 'camera'], // 可以指定来源是相册还是相机，默认二者都有
          success: function (res) {
            console.log(res)
            wx.uploadImage({
              localId: res.localIds[0], // 需要上传的图片的本地ID，由chooseImage接口获得
              isShowProgressTips: 1, // 默认为1，显示进度提示
              success: function (res) {
                // _this.$message(res.serverId)
                _this.$message({
                  message: '正在加载',
                  type: 'success'
                })
                _this.option.img = "https://shimmer.neusoft.edu.cn/wechat/web/api/media?media_id=" + res.serverId
              }
            });
          }
        })
        // else
        //   this.$message({
        //     message: '微信客户端还没准备好',
        //     type: 'warning'
        //   })
      }    }
  },
  components: {
    vueCropper
  },
  created() {
    var fullwidth = document.body.clientWidth;
    this.lastwidth = fullwidth * 0.8;
    this.lastheight = (this.lastwidth / 6) * 9;
    let _this = this;
    // this.$axios({
    //   method: 'get',
    //   url: "https://shimmer.neusoft.edu.cn/wechat/web/api/init?url=" + encodeURIComponent(location.href.split("#")[0])
    // }).then(function (res) {
    //   wx.config({
    //     debug: false,
    //     appId: res.data.appid, // 必填，公众号的唯一标识
    //     timestamp: res.data.timestamp, // 必填，生成签名的时间戳
    //     nonceStr: res.data.noncestr, // 必填，生成签名的随机串
    //     signature: res.data.signature, // 必填，签名，见附录1
    //     jsApiList: ["scanQRCode", "chooseImage", "uploadImage"] // 必填，需要使用的JS接口列表，所有JS接口列表见附录2
    //   });
    var ua = window.navigator.userAgent.toLowerCase();
    //通过正则表达式匹配ua中是否含有MicroMessenger字符串
    if (ua.match(/MicroMessenger/i) == 'micromessenger')
      this.iswechat = true;
    if (this.wechat)
      wx.ready(() => {
        _this.wxready = 1
        _this.uploadImg(null, 0)
      })
    // })
    // let _this = this;
    // this.$axios
    //   .get("wechat_jssdk.php?" + window.location.href.split("#")[0])
    //   .then(function(response) {
    //     console.log("匿名函数获取外部对象:", _this.msg);
    //     console.log("返回的sdk对象:", response);
    //     config.appId = response.data.appId;
    //     config.timestamp = response.data.timestamp;
    //     config.nonceStr = response.data.nonceStr;
    //     config.signature = response.data.signature;
    //     config.jsApiList = ["onMenuShareTimeline", "chooseImage"];
    //     wx.config(config);

    //     wx.ready(function() {
    //       console.log("微信接口配置成功:", config);
    //       console.log("获取vue变量", _this.option);
    //       var cimg;
    //       wx.chooseImage({
    //         count: 1, // 默认9
    //         sizeType: ["original", "compressed"], // 可以指定是原图还是压缩图，默认二者都有
    //         sourceType: ["album", "camera"], // 可以指定来源是相册还是相机，默认二者都有
    //         success: function(res) {
    //           alert( res.localIds[0]);
    //           cimg = res.localIds[0]; // 返回选定照片的本地ID列表，localId可以作为img标签的src属性显示图片
    //           wx.getLocalImgData({
    //             localId:cimg,
    //             success:function(res){
    //               _this.option.img = res.localData();
    //               alert(_this.option.img);
    //             }
    //           })
    //         }
    //       });
    //     });
    //   })
    //   .catch(function(error) {
    //     console.log(error);
    //   });
    // wx.error(function(res) {
    //   console.log("配置失败error:" + res);
    // });
    // console.log(wx);
    // // wx.chooseImage();
  },
  mounted() { }
};
</script>
<style scoped>
#imgbarr {
  margin: 5px 0px;
}
.showbar {
  width: 70%;
}
.fugai {
  z-index: 12;
  background-color: rgba(6, 6, 6, 0);
  position: absolute;
  width: 17.5%;
  display: block;
  height: 100%;
}
.hold {
  display: none;
}
#reust {
  position: absolute;
  background-color: #fff;
  width: 100%;
  height: 110%;
  z-index: 13;
}
.el-main {
  padding: 0;
}
.btn {
  width: 100%;
  height: 100%;
  display: block;
}
#sss {
  display: block;
  background: #a0c;
  text-align: left;
  left: 0;
  right: 0;
  margin: auto;
}

/* #cropper::before {
  content: "";
  position: absolute;
  width: 100%;
  height: 100%;
  background: url("./bg1.png") 0 0/100% 100% no-repeat;
  z-index: 2;
} */
#imgbg {
  z-index: 2;
  top: 0;
  position: absolute;
}
#zpy {
  margin-top: 5px;
}
#resut {
  width: 60%;
}
#tishi {
  color: #b21919;
  font-size: 17px;
}
#bertishi {
  background-color: rgba(6, 6, 6, 0.8);
  z-index: 20;
  height: 100vh;
  position: absolute;
  width: 100%;
}
.el-footer {
  height: auto !important;
}
.tishi {
  width: 300px;
  display: block;
  margin: 10% auto 0 auto;
  left: 0;
  right: 0;
  background: url("./tishi.png") no-repeat 0 0 /100% 100%;
  height: 500px;
}
.cropper-view-box {
  outline: 1px solid #eee !important;
}
</style>
