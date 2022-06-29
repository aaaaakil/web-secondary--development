<template>
  <div style="width: 100%; height: 100%;">
    <div v-for="item in tableData1" :key="item.orderName" @click="do_EventCenter_messageSuccess(item)">{{
        item.orderName
    }} - {{ item.orderStatus }}</div>
    <img :src="imgSrc" alt="">
  </div>
</template>

<script>
const zipObject = (arr1, arr2) => {
  const ret = {};
  arr1.forEach((item, index) => {
    ret[item] = arr2[index];
  });
  return ret;
};
import { queryAssetById } from './api/asset'
export default {
  props: {
    dataSource: {
      type: Array,
      default: () => [],
    },
    componentId: {
      type: String | undefined,
      default: "",
    },
    options: {
      type: Object,
      default: () => ({
        // 配置项从externalVariables里取
        externalVariables: {},
      }),
    },
    updateProcess: {
      type: Function,
      default: () => { },
    },
  },
  data() {
    return {
      tableData1: [],
      // imgSrc: require('./asset/客户验收.png')
      imgSrc: require('./asset/发起订单.png'),
      orderStatus: '',
      orderName: ''
    };
  },
  computed: {
    tableDataHeader() {
      return (this.dataSource[0] || []).map(t => ({
        prop: t,
        label: t,
      }));
    },
    tableData() {
      let [header, ...tableData] = this.dataSource;
      tableData = tableData || [];
      return tableData.map(d => (window?._?.zipObject || zipObject)(header, d));
    },
  },
  mounted() {
    // queryAssetById('2a898169-ae11-46aa-aa86-76cc8c04b2d4').then(res => {
    //   this.tableData1 = this.translatePlatformDataToJsonArray(res)
    //   // console.log(this.tableData1);
    // })
    this.getDataInfo()
    setInterval(() => {
      this.getDataInfo()
      this.tableData1.forEach(item => {
        if (this.orderName == item.orderName) {
          this.orderStatus = item.orderStatus
        }
      })
      // console.log(this.orderStatus);
      switch (this.orderStatus) {
        case '发起订单':
          this.imgSrc = require('./asset/发起订单.png');
          break;
        case '订单审核':
          this.imgSrc = require('./asset/订单审核.png');
          break;
        case '订单确认':
          this.imgSrc = require('./asset/订单确认.png');
          break;
        case '订单拆解':
          this.imgSrc = require('./asset/订单拆解.png');
          break;
        case '订单开发':
          this.imgSrc = require('./asset/订单拆解.png');
          break;
        case '订单集成验证':
          this.imgSrc = require('./asset/订单集成验证.png');
          break;
        case '客户验收':
          this.imgSrc = require('./asset/客户验收.png');
          break;
        case '订单完成':
          this.imgSrc = require('./asset/订单完成.png');
          break;
      }
    }, 5000)
    const events = [
      {
        key: "onClick",
        name: "点击",
        payload: [
          {
            name: "名称",
            dataType: "string",
            key: "name",
          },
        ],
      },
    ];

    const actions = [
      {
        key: "messageSuccess",
        name: "改变状态",
        params: [
          {
            key: "row",
            name: "行信息",
            dataType: "object",
          },
        ],
      },
    ];

    this.componentId &&
      window.componentCenter?.register &&
      window.componentCenter.register(this.componentId, "comp", this, {
        events,
        actions,
      });
    this.updateProcess && this.updateProcess();
  },
  methods: {
    getDataInfo() {
      queryAssetById('2a898169-ae11-46aa-aa86-76cc8c04b2d4').then(res => {
        this.tableData1 = this.translatePlatformDataToJsonArray(res)
        // console.log(this.tableData1);
      })
    },
    translatePlatformDataToJsonArray(originTableData) {
      let originTableHeader = originTableData.data[0];
      let tableHeader = [];
      originTableHeader.forEach((item) => {
        tableHeader.push(item.col_name);
      });
      let tableBody = originTableData.data[1];
      let tableData = [];
      tableBody.forEach((tableItem) => {
        let temp = {};
        tableItem.forEach((item, index) => {
          temp[tableHeader[index]] = item;
        });
        tableData.push(temp);
      });
      return tableData;
    },
    clickBt() {
      this.componentId &&
        window.eventCenter?.triggerEvent &&
        window.eventCenter.triggerEvent(this.componentId, "onClick", {
          name: "二开插件",
        });
    },
    // 逻辑控制用，不可删，return内容可改
    Event_Center_getName: () => {
      return "成功通";
    },
    do_EventCenter_messageSuccess(param) {
      // console.log(param.orderName);
      this.orderStatus = param.orderStatus
      this.orderName = param.orderName
      switch (this.orderStatus) {
        case '发起订单':
          this.imgSrc = require('./asset/发起订单.png');
          break;
        case '订单审核':
          this.imgSrc = require('./asset/订单审核.png');
          break;
        case '订单确认':
          this.imgSrc = require('./asset/订单确认.png');
          break;
        case '订单拆解':
          this.imgSrc = require('./asset/订单拆解.png');
          break;
        case '订单开发':
          this.imgSrc = require('./asset/订单拆解.png');
          break;
        case '订单集成验证':
          this.imgSrc = require('./asset/订单集成验证.png');
          break;
        case '客户验收':
          this.imgSrc = require('./asset/客户验收.png');
          break;
        case '订单完成':
          this.imgSrc = require('./asset/订单完成.png');
          break;
      }
    },
  },
};
</script>
<style scoped>
* {
  padding: 0;
  margin: 0;
}

img {
  width: 100%;
  height: 100%;
}
</style>
