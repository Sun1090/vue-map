<template>
  <div class="hello">
    <div class="map" ref="mapbox"></div>
  </div>
</template>

<script>
import echarts from "echarts";
import jsonp from "jsonp";
import "echarts/map/js/china";
// const option = {
//     xAxis: {
//         type: 'category',
//         data: ['Mon', 'Tue', 'Wed', 'Thu', 'Fri', 'Sat', 'Sun']
//     },
//     yAxis: {
//         type: 'value'
//     },
//     series: [{
//         data: [820, 932, 901, 934, 1290, 1330, 1320],
//         type: 'line'
//     }]
// };
const option = {
  title: {
    text: "新型冠状病毒肺炎",
    textStyle: {
      color: "red",
      fontSize: 24,
      fontWeight: "bold"
    },
    subtext: "疫情实时大数据报告",
    subtextStyle: {
      color: "#ccc",
      fontSize: 36,
      fontWeight: "normal"
    },
    textVerticalAlign: "auto",
    margin: 50
  },
  series: [
    {
      name: "确诊人数",
      type: "map",
      map: "china",
      label: {
        show: true,
        color: "#000",
        fontSize: 10
      },
      itemStyle: {
        areaStyle: "#83b5e7",
        borderColor: "red"
      },
      zoom: 1.3,
      //控制鼠标移动的颜色
      emphasis: {
        label: {
          color: "#000",
          fontSize: 16
        },
        itemStyle: {
          areaColor: "#ccc"
        }
      },
      data: []
    }
  ],
  visualMap: [
    {
      type: "piecewise",
      show: true,
      pieces: [
        { min: 10000 },
        { min: 1000, max: 9999 },
        { min: 100, max: 999 },
        { min: 10, max: 99 },
        { min: 1, max: 9 }
      ],
      // algin:'right'   默认left
      // orient:'horizontal'
      // showLabel:false
      inRange: {
        //symbol:'rect',//小圆圈
        color: ["#ffc0b1", "#9c0505"]
      },
      // }
      // itemStyle:{

      // }
      itemwidth: 20,
      itemHeight: 10
    }
  ],
  tooltip: {
    trigger: "item"
  },
  toolBox: {
    show: true,
    orient: "vertical",
    left: "right",
    top: "center",
    feature: {
      dataView: { readOnly: false },
      restore: {},
      saveAsImage: {}
    }
  }
};
export default {
  name: "HelloWorld",
  mounted() {
    this.getData();
    this.mychart = echarts.init(this.$refs.mapbox);
    // mychart.setOption(option)
    this.mychart.setOption(option);
  },
  methods: {
    getData() {
      jsonp(
        "https://interface.sina.cn/news/wap/fymap2020_data.d.json?_=1580892522427",
        {},
        (err, data) => {
          if (!err) {
            console.log(data);
            let list = data.data.list.map(item => ({
              name: item.name,
              value: item.value
            }));
            option.series[0].data = list;
            this.mychart.setOption(option);
          }
        }
      );
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style>
.hello {
  margin: 0;
  padding: 0;
}
.map {
  width: 580px;
  height: 580px;
  margin: 25px auto;
}
</style>
