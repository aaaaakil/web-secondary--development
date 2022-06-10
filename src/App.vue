<template>
  <div class="bigest">
    <div class="mask">
      <div class="left">
        123
      </div>
      <div class="right">
        <h1>登录注册</h1>
        <div class="rb">
          <div>
            <el-input placeholder="请输入手机号" class="mobile"></el-input>
          </div>
          <div class="yzcode">
            <div>
              <el-input placeholder="请输入图片验证码" class="mobile">
                <template slot="append">123456</template>
              </el-input>
            </div>
          </div>
          <div class="yzcode">
            <div>
              <el-input placeholder="请输入验证码" class="mobile">
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
</template>

<script>
// import appService from "@njsdata/app-sdk";
import eventActionDefine from "./components/msgCompConfig";
import "./index.css";
export default {
  name: "App",
  props: {
    customConfig: Object,
    info: Object,
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
.bigest {
  width: 100%;
  height: 100%;
  position: relative;
  background-color: #ccc;
  /* opacity: 0.8; */
}

.mask {
  text-align: center;
  width: 40%;
  height: 70%;
  position: fixed;
  left: 30%;
  top: 20%;
  background-color: #fff;
  display: flex;
}

.mobile {
  width: 80%;
}

.left {
  width: 30%;
  height: 100%;
  border: 1px solid #ccc;
  border-radius: 10px;
}

.right {
  width: 70%;
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

.getyzCode,.getyzCode1{
  color: skyblue;
}
.getyzCode1{
  font-weight:bolder;
}
.login{
  width: 80%;
  height: 60px;
  margin-top: 30px;
}
</style>
