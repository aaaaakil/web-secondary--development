<template>
  <div class="bigest">
    <div class="title">得民招聘网</div>
    <div class="big">
      <div class="left">
        <div class="ltop">
          <span>招工信息</span>
          <span>工人找活</span>
        </div>
        <div class="lbottom">
          <div></div>
          <div></div>
          <div></div>
        </div>
      </div>
      <div class="mask">
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
* {
  box-sizing: border-box;
}

.lbottom {
  height: 96%;
  background-color: #fff;
  display: flex;
  flex-flow: column;
  justify-content: space-around;
}

.lbottom>div {
  flex: 1;
  border-bottom: 1px solid #ccc;
}

.ltop {
  width: 100%;
  display: flex;
  justify-content: space-between;
}

.ltop>span {
  margin-top: 0;
  flex: 1;
  text-align: center;
  align-items: center;
  background-color: skyblue;
  color: #fff;
  border: 1px solid #fff;
  height: 30px;
  line-height: 30px;
}

.big {
  width: 100%;
  /* height: 100%; */

}

.bigest {
  width: 100%;
  height: 100%;
  position: relative;
  background-color: #ccc;
  /* opacity: 0.8; */
  text-align: center;
}

.mask {
  text-align: center;
  width: 30%;
  height: 70%;
  position: absolute;
  right: 3%;
  top: 10%;
  background-color: #fff;
  display: flex;
}

.mobile {
  width: 80%;
}

.left {
  width: 60%;
  height: 70%;
  border: 1px solid #fff;
  background-color: #fff;
  /* border-radius: 10px; */
  position: absolute;
  top: 10%;
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
