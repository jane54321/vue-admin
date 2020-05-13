<template>
  <div id="login">
    <div class="login-wrap">
      <ul class="menu-tab">
        <li
          :class="{'current': item.current}"
          v-for="item in menuTab"
          :key="item.id"
          @click="toggleMenu(item)"
        >{{item.txt}}</li>
      </ul>
      <!-- 表单的开始 -->
      <el-form :model="ruleForm" status-icon :rules="rules" ref="ruleForm" class="login-form">
        <el-form-item prop="mail" class="item-form">
          <label>邮箱</label>
          <el-input type="email" v-model="ruleForm.mail"></el-input>
        </el-form-item>
        <el-form-item prop="pass" class="item-form">
          <label>密码</label>
          <el-input type="password" v-model="ruleForm.pass" minlength="6" maxlength="20"></el-input>
        </el-form-item>
        <el-form-item prop="code" class="item-form">
          <label>验证码</label>
          <el-row :gutter="10">
            <el-col :span="15">
              <el-input type="VCode" v-model="ruleForm.code"></el-input>
            </el-col>
            <el-col :span="9" >
              <el-button type="success" class="clock" style="height: 40px">获取验证码</el-button>
            </el-col>
          </el-row>
        </el-form-item>
        <el-form-item>
          <el-button type="danger" @click="signForm('ruleForm')" class="login-btn block">登录</el-button>
        </el-form-item>
      </el-form>
    </div>
  </div>
</template>
<script>
export default {
  name: "login",
  data() {
    //   验证邮箱账号
    var formmail = (rule, value, callback) => {
        let reg = /^([a-zA-Z]|[0-9])(\w|\-)+@[a-zA-Z0-9]+\.([a-zA-Z]{2,4})$/;
      if (value === "") {
        callback(new Error("请输入用户名"));
      } else if(!reg.test(value)) {
          callback(new Error('用户名格式有误'));
          
        }else {
        //  reg.test(value);
        callback();
      }

    };
    // 验证密码
    var formpass = (rule, value, callback) => {
        let reg = /^(?!\D+$)(?![^a-zA-Z]+$)\S{6,20}$/
      if (value === "") {
        callback(new Error("请输入密码"));
        // console.log('1223');
        
      } else if (!reg.test(value)) {
          callback(new Error('密码格式为6-20位数字和字母'));
        }else {
        callback();
      }
    };
    // 验证验证码
    var formcode = (rule, value, callback) => {
        let reg = /^[a-z0-9]{6}$/;
      if (value === "") {
        callback(new Error("请输入验证码"));
      } else if(!reg.test(value)) {
          callback(new Error('请输入6位数字或者字母'));
          }else {

        callback();
      }
    };
    return {
      menuTab: [
        { txt: "登录", current: true },
        { txt: "注册", current: false }
      ],
      ruleForm: {
        pass: "",
        code: "",
        domains: [
          {
            value: ""
          }
        ],
        mail: ""
      },
      rules: {
        pass: [{ validator: formpass, trigger: "blur" }],
        code: [{ validator: formcode, trigger: "blur" }],
        mail: [{ validator: formmail, trigger: "blur" }]
      }
    };
  },
  created() {},
  // 这里是挂载完成后自动执行的
  mounted() {},
  // 写函数的地方
  methods: {
    // vue 数据驱动视图渲染 ， 而原生js 操作DOM元素
    toggleMenu(data) {
      this.menuTab.forEach((elem, index) => {
        elem.current = false;
      });
      // 高光
      data.current = true;
    },
    signForm(formName) {
      this.$refs[formName].validate(valid => {
        if (valid) {
          alert("sign in");
        } else {
          console.log("error sign in");
          return false;
        }
      });
    }
  }
};
</script>
//  scoped 是只针对本文件有效，去掉则针对全局有效
<style lang="scss" scoped>
#login {
  height: 100vh;
  background-color: #344a5f;
}
.login-wrap {
  width: 330px;
  margin: auto;
}
.menu-tab {
  text-align: center;
  li {
    display: inline-block;
    line-height: 36px;
    width: 88px;
    font-size: 14px;
    color: #fff;
    border-radius: 2px;
    cursor: pointer;
  }
  .current {
    background-color: rgba(0, 0, 0, 0.1);
  }
}
.login-form {
  margin-top: 29px;
  label {
    display: block;
    margin-bottom: 3px;
    font-size: 14px;
    color: #fff;
  }
}
.item-form {
  margin-bottom: 13px;
}
.block {
  display: block;
  width: 100%;
}
.login-btn {
  margin-top: 19px;
}
</style>

