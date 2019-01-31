<template>
  <div>
    <hs-upload-excel :upload-success="handleSuccess" :before-upload="beforeUpload"></hs-upload-excel>
    <span>第二行第二列数据：{{value}}</span>
    <div id="pie" :style="{width: '800px', height: '500px'}"></div>
  </div>
</template>
<script>
import echarts from "echarts";
import hsUploadExcel from "@/components/uploadExcel/UploadExcel";

export default {
  name: "excel-upload",
  components: {
    hsUploadExcel
  },
  data() {
    return {
      tableData: [],
      tableHeader: [],
      value: "",
      innerArr: [
        { value: 0, name: "已交付" },
        { value: 0, name: "未交付" }
      ],
      outerArr: [
        { value: 0, name: "延期交付" },
        { value: 0, name: "按时交付" },
        { value: 0, name: "已延期" },
        { value: 0, name: "未延期" },
        { value: 0, name: "可能延期" }
      ],
      myChart: null
    };
  },
  mounted() {
    const options = {
      tooltip: {
        trigger: "item",
        formatter: "{a} <br/>{b}: {c} ({d}%)"
      },
      legend: {
        orient: "vertical",
        x: "left",
        data: [
          "已交付",
          "未交付",
          "延期交付",
          "按时交付",
          "已延期",
          "未延期",
          "可能延期"
        ]
      },
      series: [
        {
          name: "",
          type: "pie",
          selectedMode: "single",
          radius: [0, "30%"],

          label: {
            normal: {
              position: "inner"
            }
          },
          labelLine: {
            normal: {
              show: false
            }
          },
          data:
            // [
            //   { value: 335, name: "直达"},
            //   { value: 679, name: "营销广告" },
            //   { value: 1548, name: "搜索引擎" }
            // ]
            this.innerArr
        },
        {
          name: "访问来源",
          type: "pie",
          radius: ["40%", "55%"],
          label: {
            normal: {
              formatter: "{a|{a}}{abg|}\n{hr|}\n  {b|{b}：}{c}  {per|{d}%}  ",
              backgroundColor: "#eee",
              borderColor: "#aaa",
              borderWidth: 1,
              borderRadius: 4,
              // shadowBlur:3,
              // shadowOffsetX: 2,
              // shadowOffsetY: 2,
              // shadowColor: '#999',
              // padding: [0, 7],
              rich: {
                a: {
                  color: "#999",
                  lineHeight: 22,
                  align: "center"
                },
                // abg: {
                //     backgroundColor: '#333',
                //     width: '100%',
                //     align: 'right',
                //     height: 22,
                //     borderRadius: [4, 4, 0, 0]
                // },
                hr: {
                  borderColor: "#aaa",
                  width: "100%",
                  borderWidth: 0.5,
                  height: 0
                },
                b: {
                  fontSize: 16,
                  lineHeight: 33
                },
                per: {
                  color: "#eee",
                  backgroundColor: "#334455",
                  padding: [2, 4],
                  borderRadius: 2
                }
              }
            }
          },
          data:
            // [
            //   { value: 335, name: "直达" },
            //   { value: 310, name: "邮件营销" },
            //   { value: 234, name: "联盟广告" },
            //   { value: 135, name: "视频广告" },
            //   { value: 1048, name: "百度" },
            //   { value: 251, name: "谷歌" },
            //   { value: 147, name: "必应" },
            //   { value: 102, name: "其他" }
            // ]
            this.outerArr
        }
      ]
    };
    this.myChart = echarts.init(document.getElementById("pie"));
    this.myChart.setOption(options);
  },
  methods: {
    beforeUpload(file) {
      const isLt1M = file.size / 1024 / 1024 < 2;

      if (isLt1M) {
        return true;
      }

      this.$message({
        message: "Please do not upload files larger than 2m in size.",
        type: "warning"
      });
      return false;
    },
    handleSuccess({ results, header }) {
      // console.log(results)
      this.tableData = results;
      this.tableHeader = header;
      this.value = this.tableData[1][this.tableHeader[1]];
      for (let i = 0, l = results.length; i < l; i++) {
        if ('DLV' === results[i].Status) {
          this.innerArr[0].value++;
        } else {
          this.innerArr[1].value++;
        }
        
      }
      const options = this.myChart.getOption();
      
      options.series[0].data = this.innerArr;
      console.log(options)
      this.myChart.setOption(options);
    }
  }
};
</script>
