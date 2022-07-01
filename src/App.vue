<template>
  <div style="width: 100%; height: 100%;">
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
      // imgSrc: require('./images/客户验收.png')
      imgSrc: require('../images/orderStart.png'),
      orderStatus: '',
      orderName: '',
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
    this.getDataInfo()
    setInterval(() => {
      this.getDataInfo()
      this.tableData1.forEach(item => {
        if (this.orderName == item.order_name) {
          this.orderStatus = item.business_status
        }
      })
      // console.log(this.orderStatus);
      this.changeStatus()
    }, 5000)
    // const events = [
    //   {
    //     key: "onClick",
    //     name: "点击",
    //     payload: [
    //       {
    //         name: "名称",
    //         dataType: "string",
    //         key: "name",
    //       },
    //     ],
    //   },
    // ];

    const actions = [
      {
        key: "getRow",
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
      this.tableData1 = this.all(this.dataSource)
      // console.log(this.tableData1);
    },
    all(a) {
      let arr = a
      let arr2 = []
      let headList = arr[0]
      arr.forEach((d, i) => {
        if (i !== 0) {
          let obj = {}
          headList.forEach((item, index) => {
            obj[headList[index]] = arr[i][index]
          })
          arr2.push(obj)
        }
      })
      return arr2
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
    do_EventCenter_getRow(param) {
      // {
      //   row:{}
      // }
      console.log({ "产品订单名称": param.row.order_name, "客户名称": param.client_name, "订单状态": this.changeTime(param.business_status), "期望交付时间": new Date(param.expected_delivery_time).toLocaleString(), "计划交付时间": param.plan_delivery_time ? new Date(param.plan_delivery_time).toLocaleString() : '还未确定交付时间' })
      // console.log(param.orderName);
      this.orderStatus = param.row.business_status
      this.orderName = param.row.order_name
      this.changeStatus()
    },
    changeTime(a) {
      switch (a) {
        case '1':
          return '部长初审中';
        case '2':
          return '厂长审核中'
        case '3':
          return '订单确认中'
        case '4':
          return '订单拆解中'
        case '5':
          return '订单开发中'
        case '6':
          return '集成验证中'
        case '7':
          return '订单验收中'
        case '8':
          return '已完成'
        case '9':
          return '部长初审不通过'
        case '10':
          return '厂长审核不通过'
      }
    },
    changeStatus() {
      switch (this.orderStatus) {
        case '1':
          this.imgSrc = require('../images/orderStart.png');
          break;
        case '2':
          this.imgSrc = require('../images/orderCheck.png');
          break;
        case '3':
          this.imgSrc = require('../images/orderQR.png');
          break;
        case '4':
          this.imgSrc = require('../images/orderCJ.png');
          break;
        case '5':
          this.imgSrc = require('../images/orderDevelop.png');
          break;
        case '6':
          this.imgSrc = require('../images/orderAllCheck.png');
          break;
        case '7':
          this.imgSrc = require('../images/custormQR.png');
          break;
        case '8':
          this.imgSrc = require('../images/orderOver.png');
          break;
        case '9':
          this.imgSrc = require('../images/orderCheck.png');
          break;
        case '10':
          this.imgSrc = require('../images/orderCheck.png');
          break;
      }
    }
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
