<template>
  <div class="brand-container">
    <div class="wrap">
      <header>
        <div class="weather">
          <img :src="imgSrc" />
          <span class="tem">{{ weatcherData.tem }}°C</span>
          <span class="wea">{{ weatcherData.wea }}</span>
        </div>
        <h2>新智可视化大屏Demo</h2>
        <div class="showTime">
          <span class="time">{{ nowTime }}</span>
          <span class="date">
            <span>{{ week }}</span>
            <span>{{ date }}</span>
          </span>
        </div>
      </header>

      <section class="mainbox">
        <div class="item left">
          <!-- <div class="panel"> -->
          <rotateColorfulCom />
          <!-- <div class="panel-footer"></div>
          </div> -->
          <waterPoloCom />
          <pyramidTrendCom />
        </div>

        <div class="item center">
          <div class="resume">
            <div class="resume-hd">
              <ul>
                <li>
                  <countTo
                    :startVal="startVal"
                    :endVal="490"
                    :duration="6000"
                    separator=""
                  ></countTo>
                </li>
                <li>
                  <countTo
                    :startVal="startVal"
                    :endVal="75"
                    :duration="6000"
                    separator=""
                  ></countTo>
                </li>
                <li>
                  <countTo
                    :startVal="startVal"
                    :endVal="3000"
                    :duration="6000"
                    separator=""
                  ></countTo>
                </li>
              </ul>
            </div>
            <div class="resume-bd">
              <ul>
                <li>公司总人数（单位：人）</li>
                <li>技术人员占比（单位：%）</li>
                <li>产品投资额（单位：万元）</li>
              </ul>
            </div>
          </div>
          <div class="map">
            <div class="chart" id="chart_map"></div>
            <div class="map1"></div>
            <div class="map2"></div>
            <div class="map3"></div>
          </div>
        </div>

        <div class="item right">
          <div class="panel">
            <h2>营收状况</h2>
            <income />
            <div class="panel-footer"></div>
          </div>
          <div class="panel">
            <h2>客户分布</h2>
            <distribution />
            <div class="panel-footer"></div>
          </div>
          <div class="panel">
            <h2>发展历程</h2>
            <history />
            <div class="panel-footer"></div>
          </div>
        </div>
      </section>
    </div>
  </div>
</template>

<script>
import "@/assets/js/flexible";
import "@/assets/js/china";
import countTo from "vue-count-to";

export default {
  name: "Brand",
  components: {
    countTo,
  },
  data() {
    return {
      nowTime: "",
      week: "",
      date: "",
      timer: null,
      imgSrc: "",
      weatcherData: {},
      startVal: 0,
      geoCoordMap: {},
      XAData: [
        [{ name: "长沙" }, { name: "北京", value: 100 }],
        [{ name: "长沙" }, { name: "上海", value: 100 }],
        [{ name: "长沙" }, { name: "广州", value: 100 }],
        [{ name: "长沙" }, { name: "深圳", value: 100 }],
        [{ name: "长沙" }, { name: "西安", value: 100 }],
      ],
      XNData: [
        [{ name: "长沙" }, { name: "西宁", value: 100 }],
        [{ name: "长沙" }, { name: "拉萨", value: 100 }],
        [{ name: "长沙" }, { name: "哈尔滨", value: 100 }],
        [{ name: "长沙" }, { name: "成都", value: 100 }],
        [{ name: "长沙" }, { name: "重庆", value: 100 }],
      ],
      YCData: [
        [{ name: "北京" }, { name: "长沙", value: 100 }],
        [{ name: "北京" }, { name: "贵阳", value: 100 }],
        [{ name: "北京" }, { name: "杭州", value: 100 }],
      ],
    };
  },
  computed: {},
  created() {},
  mounted() {
    this.getWeather();
    this.timer = setInterval(() => {
      this.getWeather();
    }, 1000 * 60 * 60);

    this.nowTimes();
  },
  methods: {
    timeFormate(timeStamp) {
      //显示当前时间
      let newDate = new Date(timeStamp);
      let year = newDate.getFullYear();
      let month =
        newDate.getMonth() + 1 < 10
          ? "0" + (newDate.getMonth() + 1)
          : newDate.getMonth() + 1;
      let date =
        newDate.getDate() < 10 ? "0" + newDate.getDate() : newDate.getDate();
      let hh =
        newDate.getHours() < 10 ? "0" + newDate.getHours() : newDate.getHours();
      let mm =
        newDate.getMinutes() < 10
          ? "0" + newDate.getMinutes()
          : newDate.getMinutes();
      let ss =
        newDate.getSeconds() < 10
          ? "0" + newDate.getSeconds()
          : newDate.getSeconds();
      let week = newDate.getDay();
      let weeks = ["日", "一", "二", "三", "四", "五", "六"];
      let getWeek = "星期" + weeks[week];
      this.week = getWeek;
      this.date = year + "." + month + "." + date;
      this.nowTime = hh + ":" + mm + ":" + ss;
    },
    nowTimes() {
      this.timeFormate(new Date());
      setInterval(this.nowTimes, 1000);
      this.clear();
    },
    clear() {
      clearInterval(this.nowTimes);
      this.nowTimes = null;
    },
    getWeather() {
      // 第三方天气api接口
      axios
        .get("https://www.tianqiapi.com/api/", {
          params: {
            appid: "26148275",
            appsecret: "2id6H48Y",
            version: "v6",
          },
        })
        .then((res) => {
          if (res.data) {
            if (res.data.wea_img == "xue") {
              this.imgSrc = require("../assets/img/brand/xue.png");
            } else if (res.data.wea_img == "yin") {
              this.imgSrc = require("../assets/img/brand/yin.png");
            } else if (
              res.data.wea_img == "yu" ||
              res.data.wea_img == "bingbao"
            ) {
              this.imgSrc = require("../assets/img/brand/yu.png");
            } else if (res.data.wea_img == "yun") {
              this.imgSrc = require("../assets/img/brand/yun.png");
            } else if (res.data.wea_img == "wu") {
              this.imgSrc = require("../assets/img/brand/wu.png");
            } else if (res.data.wea_img == "shachen") {
              this.imgSrc = require("../assets/img/brand/shachen.png");
            } else if (res.data.wea_img == "lei") {
              this.imgSrc = require("../assets/img/brand/lei.png");
            } else {
              this.imgSrc = require("../assets/img/brand/qing.png");
            }
            this.weatcherData = res.data;
          }
        })
        .catch((err) => {
          console.log(err);
        });
    },
    convertData(data) {
      // 地图数据转换
      let res = [];
      for (let i = 0; i < data.length; i++) {
        let dataItem = data[i];
        let fromCoord = this.geoCoordMap[dataItem[0].name];
        let toCoord = this.geoCoordMap[dataItem[1].name];
        if (fromCoord && toCoord) {
          res.push({
            fromName: dataItem[0].name,
            toName: dataItem[1].name,
            coords: [fromCoord, toCoord],
            value: dataItem[1].value,
          });
        }
      }
      return res;
    },
  },
  beforeDestroy() {
    clearInterval(this.timer);
  },
};
</script>

<style lang="scss" scoped>
.brand-container {
  position: absolute;
  width: 100%;
  height: 100%;
  background: #000;
  .wrap {
    background: url(../assets/img/brand/bg.jpg) no-repeat #000;
    background-size: cover;
    line-height: 1.15;
    header {
      position: relative;
      height: 1rem;
      background: url(../assets/img/brand/head_bg.png) no-repeat top center;
      background-size: 100% 100%;
      h2 {
        color: #7ef0ff;
        font-size: 0.475rem;
        text-align: center;
        line-height: 0.75rem;
        letter-spacing: 1px;
      }
      .weather {
        position: absolute;
        left: 1.375rem;
        top: 0.35rem;
        font-size: 0.25rem;
        color: rgba(126, 240, 255, 0.7);
        img {
          width: 0.45rem;
        }
        span {
          display: inline-block;
        }
        .tem {
          margin: 0 0.1rem 0 0.2rem;
        }
      }
      .showTime {
        position: absolute;
        right: 1.375rem;
        top: 0.5rem;
        color: rgba(126, 240, 255, 0.7);
        display: flex;
        .time {
          font-size: 0.28rem;
          margin-right: 0.18rem;
        }
        .date {
          span {
            display: block;
            &:nth-child(1) {
              font-size: 0.12rem;
              text-align: right;
            }
            &:nth-child(2) {
              font-size: 0.14rem;
            }
          }
        }
      }
    }

    .mainbox {
      min-width: 1024px;
      max-width: 1920px;
      padding: 0.125rem 0.125rem 0;
      display: flex;
      .item {
        flex: 3;

        &.center {
          flex: 5;
          margin: 0 0.125rem 0.1rem;
          overflow: hidden;

          .resume {
            background: rgba(101, 132, 226, 0.1);
            padding: 0.1875rem;
            .resume-hd {
              position: relative;
              border: 1px solid rgba(25, 186, 139, 0.17);
              ul {
                display: flex;
                %li-line {
                  content: "";
                  position: absolute;
                  height: 50%;
                  width: 1px;
                  background: rgba(255, 255, 255, 0.2);
                  top: 25%;
                }
                li {
                  position: relative;
                  flex: 1;
                  text-align: center;
                  height: 1.2rem;
                  line-height: 1.2rem;
                  font-size: 0.65rem;
                  color: #ffeb7b;
                  padding: 0.05rem 0;
                  font-family: "DIGITALDREAMFAT";
                  font-weight: bold;
                  &:nth-child(2) {
                    &:after {
                      @extend %li-line;
                      right: 0;
                    }
                    &:before {
                      @extend %li-line;
                      left: 0;
                    }
                  }
                }
              }
              &:before {
                content: "";
                position: absolute;
                width: 30px;
                height: 10px;
                border-top: 2px solid #02a6b5;
                border-left: 2px solid #02a6b5;
                top: 0;
                left: 0;
              }
              &:after {
                content: "";
                position: absolute;
                width: 30px;
                height: 10px;
                border-bottom: 2px solid #02a6b5;
                border-right: 2px solid #02a6b5;
                right: 0;
                bottom: 0;
              }
            }
            .resume-bd {
              ul {
                display: flex;
                li {
                  flex: 1;
                  height: 0.5rem;
                  line-height: 0.5rem;
                  text-align: center;
                  font-size: 0.225rem;
                  color: rgba(255, 255, 255, 0.7);
                  padding-top: 0.125rem;
                }
              }
            }
          }
        }

        %map-style {
          position: absolute;
          top: 50%;
          left: 50%;
          transform: translate(-50%, -50%);
          width: 6.475rem;
          height: 6.475rem;
          background: url(../assets/img/brand/map.png) no-repeat;
          background-size: 100% 100%;
          opacity: 0.3;
        }
        .map {
          position: relative;
          height: 10.125rem;
          .chart {
            position: absolute;
            top: 0;
            left: 0;
            z-index: 5;
            height: 10.125rem;
            width: 100%;
          }
          .map1 {
            @extend %map-style;
          }
          .map2 {
            @extend %map-style;
            width: 8.0375rem;
            height: 8.0375rem;
            background-image: url(../assets/img/brand/lbx.png);
            opacity: 0.6;
            -webkit-animation: rotate 15s linear infinite;
            animation: rotate 15s linear infinite;
            z-index: 2;
          }
          .map3 {
            @extend %map-style;
            width: 7.075rem;
            height: 7.075rem;
            background-image: url(../assets/img/brand/jt.png);
            -webkit-animation: rotate1 10s linear infinite;
            animation: rotate1 10s linear infinite;
          }
        }
        .panel {
          position: relative;
          height: 3.875rem;
          border: 1px solid rgba(25, 186, 139, 0.17);
          background: rgba(255, 255, 255, 0.04)
            url(../assets/img/brand/line.png);
          padding: 0 0.1875rem 0;
          margin-bottom: 0.1875rem;
          &:before {
            position: absolute;
            top: 0;
            left: 0;
            content: "";
            width: 10px;
            height: 10px;
            border-top: 2px solid #02a6b5;
            border-left: 2px solid #02a6b5;
          }
          &:after {
            position: absolute;
            top: 0;
            right: 0;
            content: "";
            width: 10px;
            height: 10px;
            border-top: 2px solid #02a6b5;
            border-right: 2px solid #02a6b5;
          }

          .panel-footer {
            position: absolute;
            left: 0;
            bottom: 0;
            width: 100%;
            &:before {
              position: absolute;
              bottom: 0;
              left: 0;
              content: "";
              width: 10px;
              height: 10px;
              border-bottom: 2px solid #02a6b5;
              border-left: 2px solid #02a6b5;
            }
            &:after {
              position: absolute;
              bottom: 0;
              right: 0;
              content: "";
              width: 10px;
              height: 10px;
              border-bottom: 2px solid #02a6b5;
              border-right: 2px solid #02a6b5;
            }
          }

          h2 {
            height: 0.6rem;
            line-height: 0.6rem;
            text-align: center;
            color: #fff;
            font-size: 0.225rem;
            font-weight: 400;
            a {
              margin: 0 0.1875rem;
              color: #fff;
              text-decoration: none;
            }
          }
          .chart {
            height: 3rem;
          }
        }
      }
    }
  }
}

@-webkit-keyframes rotate {
  from {
    transform: translate(-50%, -50%) rotate(0deg);
  }
  to {
    transform: translate(-50%, -50%) rotate(360deg);
  }
}
@keyframes rotate {
  from {
    transform: translate(-50%, -50%) rotate(0deg);
  }
  to {
    transform: translate(-50%, -50%) rotate(360deg);
  }
}

@-webkit-keyframes rotate1 {
  from {
    transform: translate(-50%, -50%) rotate(0deg);
  }
  to {
    transform: translate(-50%, -50%) rotate(-360deg);
  }
}
@keyframes rotate1 {
  from {
    transform: translate(-50%, -50%) rotate(0deg);
  }
  to {
    transform: translate(-50%, -50%) rotate(-360deg);
  }
}

@media screen and (max-width: 1024px) {
  html {
    font-size: 42px !important;
  }
}
@media screen and (min-width: 1920) {
  html {
    font-size: 80px !important;
  }
}
</style>
