<template>
  <div class="login-wrap">
    <div class="ms-login">
      <div class="ms-title">后台管理系统</div>
      <el-form :model="formModel" ref="login" label-width="0px" class="ms-content">
        <el-form-item prop="username">
          <el-input v-model="formModel.user" placeholder="请输入账号">
            <el-button slot="prepend" icon="el-icon-user"></el-button>
          </el-input>
        </el-form-item>
        <el-form-item prop="password">
          <el-input show-password v-model="formModel.password" placeholder="请输入密码">
            <el-button slot="prepend" icon="el-icon-lock"></el-button>
          </el-input>
        </el-form-item>
        <el-form-item prop="checkCode">
          <el-row>
            <el-col :span="15">
              <el-input placeholder="请输入验证码" v-model="formModel.checkCode"></el-input>
            </el-col>
            <el-col :span="8" :offset="1">
              <identify :contentWidth="96" :contentHeight="40" @changeCode="getCheckCode" ref="checkCodeImg"></identify>
            </el-col>
          </el-row>
        </el-form-item>
        <div class="login-btn">
          <el-button type="primary" @click="loadBtn()">登录</el-button>
        </div>
      </el-form>
    </div>
  </div>
</template>
<script>

import identify from "@/components/identify";

export default {
  components: {
    identify,
  },
  data() {
    return {
      formModel: {
        user: "",
        password: "",
        checkCode: ""
      },
      rules: {
        username: [
          { required: true, message: "请输入用户名", trigger: "blur" }
        ],
        password: [{ required: true, message: "请输入密码", trigger: "blur" }],
        checkCode: [
          { required: true, message: "请输入验证码", trigger: "blur" }
        ]
      },
      checkNum: "" //验证码组件返回的随机数
    };
  },
  created() {
    //回车键登录 只在登录页面有效
    let that = this;
    document.onkeydown = function (e) {
      let keyCode = window.event.keyCode;
      if (that.$route.path == "/login" && keyCode == 13) {
        //验证在登录界面和按得键是回车键enter
        that.loadBtn(); //登录函数
      }
    };
  },
  methods: {
    loadBtn() {      //验证
      if (!this.formModel.user) {
        this.$message.warning("用户名不能为空");
        return;
      }
      if (!this.formModel.password) {
        this.$message.warning("密码不能为空");
        return;
      }
      if (!this.formModel.checkCode) {
        this.$message.warning("验证码不能为空");
        return;
      }
      if (this.formModel.checkCode != this.checkNum) {
        this.$message.warning("验证码不正确");
        //刷新验证码并清空输入
        this.refreshCode();
        return;
      }
      if (this.formModel.user == "admin" && this.formModel.password == "123") {
        this.$message.success("登录成功");
        this.$router.push({
          path: "/Home"
        });
      } else {
        this.$message.error("账号或密码错误！请重新输入！");
        //刷新验证码并清空输入
        this.refreshCode();
        return;
      }
      //   var pData = {
      //     Admin_Code: this.formModel.user,
      //     Admin_Pwd: this.formModel.password
      //   };
      //   ajax
      //     .post(apiUrls.login, pData, { nocrypt: true })
      //     .then(r => {
      //       console.log(r);
      //       if (!r.data.success) {
      //         this.$message.error(r.data.returnMsg);

      //         //刷新验证码并清空输入
      //         this.refreshCode();
      //         return;
      //       }
      //       this.$message.success("登录成功");

      //       storage.session.set("userRole", this.formModel.user);
      //       storage.session.set("user", JSON.stringify(r.data.returnData));

      //       this.$router.push({
      //         path: "/home"
      //       });
      //     })
      //     .catch(err => {
      //       this.$message.error("登录错误,请联系工作人员");

      //       //刷新验证码并清空输入
      //       this.refreshCode();
      //     });
    },
    //获取验证码返回的随机数
    getCheckCode(num) {
      this.checkNum = num;
    },
    //刷新验证码
    refreshCode() {
      this.$refs.checkCodeImg.handleClick();
      this.formModel.checkCode = "";
    }
  },
  mounted() { }
};
</script>
<style scoped>
.login-wrap {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-image: url("../assets/loginss.jpg");
  background-size: 100%;
  background-size: cover;
  /* background: rgba(0, 58, 108, 1); */
}

.ms-title {
  width: 100%;
  line-height: 50px;
  text-align: center;
  font-size: 20px;
  color: #fff;
  border-bottom: 1px solid #ddd;
}

.ms-login {
  position: absolute;
  left: 50%;
  top: 50%;
  width: 350px;
  margin: -190px 0 0 -175px;
  border-radius: 5px;
  background: rgba(255, 255, 255, 0.3);
  overflow: hidden;
}

.ms-content {
  padding: 30px 30px;
}

.login-btn {
  text-align: center;
}

.login-btn button {
  width: 100%;
  height: 36px;
  margin-bottom: 10px;
}

.login-tips {
  font-size: 12px;
  line-height: 30px;
  color: #fff;
}
</style>
