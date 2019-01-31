<template>
  <div>
    <hs-upload-excel :upload-success="handleSuccess" :before-upload="beforeUpload"></hs-upload-excel>
    <span>第二行第二列数据：{{value}}</span>
    <div id="pie" :style="{width: '500px', height: '500px'}"></div>
  </div>
</template>
<script>
import echarts from 'echarts'
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
      value: ""
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
          "直达",
          "营销广告",
          "搜索引擎",
          "邮件营销",
          "联盟广告",
          "视频广告",
          "百度",
          "谷歌",
          "必应",
          "其他"
        ]
      },
      series: [
        {
          name: "访问来源",
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
          data: [
            { value: 335, name: "直达", selected: true },
            { value: 679, name: "营销广告" },
            { value: 1548, name: "搜索引擎" }
          ]
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
          data: [
            { value: 335, name: "直达" },
            { value: 310, name: "邮件营销" },
            { value: 234, name: "联盟广告" },
            { value: 135, name: "视频广告" },
            { value: 1048, name: "百度" },
            { value: 251, name: "谷歌" },
            { value: 147, name: "必应" },
            { value: 102, name: "其他" }
          ]
        }
      ]
    };
    let myChart = echarts.init(document.getElementById('pie'));
    myChart.setOption(options);
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
      this.tableData = results;
      this.tableHeader = header;
      this.value = this.tableData[1][this.tableHeader[1]];
    }
  }
};
</script>
