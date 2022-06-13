<template>
  <div class="bigest">
    <div class="title">得民招聘网</div>
    <div class="big">
      <div class="left">
        <div class="ltop">
          <span v-for="item in this.titleData" :key="item">{{ item }}</span>
        </div>
        <div class="lbottom">
          <div v-for="item in this.Data">{{ item }}</div>
        </div>
      </div>
      <div class="mask">
        <div class="right">
          <h1>登录注册</h1>
          <div class="rb">
            <div>
              <el-input placeholder="请输入手机号" class="mobile" v-model="mobile"></el-input>
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
      titleData: [],
      textData: [],
      Data: [],
      x: []
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
    queryAssetById('29ba12cb-4b5f-4c0c-9564-4374fedba8cd').then(res => {
      console.log(res.data);
      res.data[0].map(item => {
        this.titleData.push(item.col_name)
      })
      res.data[1].map(item => {
        this.textData.push(item)
      })
      this.Data = this.textData.splice(0, 3)
    })
  },
  methods: {
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

.imgyzm {
  cursor: pointer;
}

.lbottom {
  height: 93%;
  display: flex;
  flex-flow: column;
  justify-content: space-around;
  background-color: #fff;

}

.lbottom>div {
  flex: 1;
  border-bottom: 1px solid #ccc;
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
  text-align: center;
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
}
</style>
