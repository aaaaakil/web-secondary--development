<template>
  <div class="bigest">
    <div class="title">得民招聘网</div>
    <div class="big">
      <div class="left">
        <div class="ltop">
          <span @click="changeTab">招工信息</span>
          <span @click="changeTab1">工人找活</span>
        </div>
        <div class="lbottom">
          <div v-if="this.contentData">
            <div v-for="item in this.contentData" class="content">
              <div class="content-left">
                <div class="ctitle">{{ item.title }}</div>
                <div class="ccontent">{{ item.content }}</div>
                <div class="cperson">联系人：{{ item.person }}</div>
              </div>
              <div class="row">
                <div class="red">正在招</div>
                <div class="ccontent">江苏南京</div>
                <el-button type="primary">立即联系</el-button>
              </div>
            </div>
          </div>
          <div v-if="this.userData" class="">
            <div v-for="item in this.userData" class="content1">
              <div>
                123
              </div>
              <div class="row">
                <div>
                  <div class="ctitle">{{ item.name }}</div>
                  <div class="ccontent">{{ item.sex }}-{{ item.age }}岁-{{ item.nation }}族-工龄{{ item.seniority }}年</div>
                </div>
                <div class="cperson">{{ item.introduce }}</div>
              </div>
              <div class="row">
                <div class="ccontent">江苏南京</div>
                <el-button type="primary">立即联系</el-button>
              </div>
            </div>
          </div>
        </div>
      </div>
      <div class="mask">
        <div class="right">
          <h1>登录注册</h1>
          <div class="rb">
            <div>
              <el-input placeholder="请输入手机号" class="mobile" v-model="mobile"></el-input>
            </div>
            <div>
              <el-input placeholder="请输入姓名" class="mobile" v-model="name"></el-input>
            </div>
            <div class="yzcode">
              <div>
                <el-input placeholder="请输入图片验证码" class="mobile" v-model="yzm">
                  <template slot="append"><span class="imgyzm">123456</span></template>
                </el-input>
              </div>
            </div>
            <div class="yzcode">
              <div>
                <el-input placeholder="请输入验证码" class="mobile" v-model="sjyzm">
                  <template slot="append"><span class="getyzCode">获取验证码</span></template>
                </el-input>
              </div>
            </div>
            <div>
              <input type="checkbox">已阅读并同意<span class="getyzCode1">《隐私政策》《服务协议》</span>
              <el-button type="primary" class="login">登录/注册</el-button>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
// import appService from "@njsdata/app-sdk";
import eventActionDefine from "./components/msgCompConfig";
import "./index.css";
import { queryAssetById } from "./api/asset"
export default {
  name: "App",
  props: {
    customConfig: Object,
    info: Object,
  },
  data() {
    return {
      mobile: '',
      yzm: '',
      sjyzm: '',
      name: '',
      allData: [],
      newAllData: [],
      contentData: [],
      userData: []
    }
  },
  computed: {
    title() {
      return this.customConfig?.title || "数据构建";
    },
    desc() {
      return this.customConfig?.desc || "描述";
    },
  },
  mounted() {
    let { componentId } = this.customConfig || {};
    componentId &&
      window.componentCenter?.register(
        componentId,
        "comp",
        this,
        eventActionDefine
      );
    //请求数据图书馆的数据
    queryAssetById('29ba12cb-4b5f-4c0c-9564-4374fedba8cd').then(res => {
      // console.log(res.data);
      this.allData = this.translatePlatformDataToJsonArray(res)
      this.contentData = this.translatePlatformDataToJsonArray(res)
    })
    queryAssetById('95667b34-c650-4046-8be4-f75973b27697').then(res => {
      // console.log(res.data);
      this.newAllData = this.translatePlatformDataToJsonArray(res)
      // this.userData = this.translatePlatformDataToJsonArray(res)
    })
  },
  methods: {
    //处理数据
    //  将平台返回数据转化为对象数组的形式 
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
      // console.log(tableData);
      return tableData;
    },
    //tab切换
    changeTab() {
      this.userData = []
      this.contentData = this.allData
      // console.log(this.contentData);
    },
    changeTab1() {
      this.contentData = []
      this.userData = this.newAllData
      // console.log(this.userData);
    },
    goToStudy() {
      window.open(this.customConfig?.url || "http://baidu.com");
    },
    getData() {
      //   console.log(appService.getMenuData(), "菜单");
      //   console.log(appService.getPageData(), "页面");
      //   console.log(appService.getVariable(), "变量");
    },
    triggerEvent() {
      let { componentId, appId } = this.customConfig || {};
      componentId &&
        appId &&
        window.eventCenter?.triggerEventNew({
          objectId: appId,
          componentId: componentId,
          type: "app",
          event: "onImgClick",
          payload: {
            value: "sasdasd",
          },
        });
    },
    do_EventCenter_messageSuccess() {
      alert("动作执行成功！");
    },
    Event_Center_getName() {
      return "登录页面";
    },
  },
  destroyed() {
    window.componentCenter?.removeInstance(this.customConfig?.componentId);
  },
};
</script>
<style scoped>
* {
  box-sizing: border-box;
}

.cneter {
  display: flex;
  justify-content: space-between;
}

.imgyzm {
  cursor: pointer;
}

.row {
  display: flex;
  flex-direction: column;
  justify-content: space-around;
}

.content-left {
  height: 50%;
  width: 70%;
  overflow: hidden;
  word-wrap: break-word;
  text-overflow: ellipsis;
  display: flex;
  flex-direction: column;
  justify-content: space-around;
}

.ctitle {
  font-weight: bolder;
  font-size: 16px;
  display: -webkit-box;
  -webkit-box-orient: vertical;
  -webkit-line-clamp: 2;
  overflow: hidden;
  margin-top: 15px;
}

.ccontent {
  font-size: 16px;
  color: #c2c2c3;
}

.cperson {
  margin: 10px 0;
  font-size: 16px;
  font-weight: bolder;
}

.content {
  height: 90%;
  width: 100%;
  display: flex;
  justify-content: space-between;
  border-bottom: 1px solid #000;
}

.red {
  color: red;
  font-weight: bolder;
  font-size: 20px;
}

.content1 {
  height: 200px;
  display: flex;
  justify-content: space-around;
  border-bottom: 1px solid #000;
}

.lbottom {
  height: 93%;
  /* display: flex; */
  /* flex-flow: column; */
  /* justify-content: space-around; */
  background-color: #fff;
  overflow: auto;

}

.lbottom>div {
  flex: 1;
  overflow: hidden;
  text-overflow: ellipsis;
  border-bottom: 1px solid #ccc;
  font-size: 25px;
}

.ltop {
  width: 25%;
  height: 30px;
  display: flex;
  justify-content: space-between;
  margin-bottom: 20px;
}

.ltop>span {
  margin-top: 0;
  flex: 1;
  text-align: center;
  align-items: center;
  background-color: #169bd4;
  color: #fff;
  /* border: 1px solid #fff; */
  height: 30px;
  line-height: 30px;
  cursor: pointer;
  margin-right: 20px;
  border-radius: 5px;
}

.big {
  width: 100%;
  /* height: 100%; */

}

.bigest {
  width: 100%;
  height: 100%;
  position: relative;
  /* background-color: #ccc;
   */
  background-color: #f2f2f2;
  /* opacity: 0.8; */
  /* text-align: center; */
}

.mask {
  text-align: center;
  width: 30%;
  height: 70%;
  position: absolute;
  right: 3%;
  top: 20%;
  background-color: #fff;
  display: flex;
}

.mobile {
  width: 80%;
}

.left {
  width: 60%;
  height: 70%;
  /* border: 1px solid #fff; */
  /* border-radius: 10px; */
  position: absolute;
  top: 20%;
  left: 5%;
}

.right {
  width: 50%;
  height: 100%;
  flex: 1;
}

.rb {
  width: 100%;
  height: 60%;
  margin-top: 100px;
  display: flex;
  flex-direction: column;
  justify-content: space-around;
}

.getyzCode,
.getyzCode1 {
  color: skyblue;
  cursor: pointer;
}

.getyzCode1 {
  font-weight: bolder;
}

.login {
  width: 80%;
  height: 60px;
  margin-top: 30px;
}

.title {
  font-size: 50px;
  font-weight: bolder;
  text-align: center;
}
</style>
