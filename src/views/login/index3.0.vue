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

        <el-form-item prop="repass" class="item-form" v-show="model === 'register'">
          <label>确认密码</label>
          <el-input type="password" v-model="ruleForm.repass"></el-input>
        </el-form-item>

        <el-form-item prop="code" class="item-form">
          <label>验证码</label>
          <el-row :gutter="10">
            <el-col :span="15">
              <el-input type="VCode" v-model="ruleForm.code"></el-input>
            </el-col>
            <el-col :span="9">
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
import { reactive, ref, isRef, toRefs, onMounted } from "@vue/composition-api";
import {
  stripscript,
  validateEmail,
  validatePasswd,
  validateVCode
} from "@/utils/validate";
export default {
  name: "login",
  setup(props, { refs }) {
    //   验证邮箱账号
    var formmail = (rule, value, callback) => {
      // let reg = /^([a-zA-Z]|[0-9])(\w|\-)+@[a-zA-Z0-9]+\.([a-zA-Z]{2,4})$/;
      if (value === "") {
        callback(new Error("请输入用户名"));
      } else if (validateEmail(value)) {
        callback(new Error("用户名格式有误"));
      } else {
        //  reg.test(value);
        callback();
      }
    };
    // 验证密码
    var formpass = (rule, value, callback) => {
      // console.log(stripscript(value));
      // 过滤后的数据
      ruleForm.pass = stripscript(value);
      value = ruleForm.pass;
      // let reg = /^(?!\D+$)(?![^a-zA-Z]+$)\S{6,20}$/
      if (value === "") {
        callback(new Error("请输入密码"));
      } else if (validatePasswd(value)) {
        callback(new Error("密码格式为6-20位数字和字母"));
      } else {
        callback();
      }
    };
    // 验证确认密码
    var formrepass = (rule, value, callback) => {
      // 使用v-show是要在加一个判断，v-if不需要
      if (model.value === "login") {
        callback();
      }
      // 过滤后的数据
      ruleForm.repass = stripscript(value);
      value = ruleForm.repass;
      if (value === "") {
        callback(new Error("请再次输入密码"));
      } else if (value != ruleForm.pass) {
        callback(new Error("请确认密码输入是否正确"));
      } else {
        callback();
      }
    };
    // 验证验证码
    var formcode = (rule, value, callback) => {
      // let reg = /^[a-z0-9]{6}$/;
      if (value === "") {
        callback(new Error("请输入验证码"));
      } else if (validateVCode(value)) {
        callback(new Error("请输入6位数字或者字母"));
      } else {
        callback();
      }
    };
    /**
     * 声明数据
     * */

    //   这里放置data数据， 生命周期，自定义的函数
    const menuTab = reactive([
      { txt: "登录", current: true, type: "login" },
      { txt: "注册", current: false, type: "register" }
    ]);
    //   模块值
    const model = ref("login");
    // 表单绑定数据
    const ruleForm = reactive({
      mail: "",
      pass: "",
      repass: "",
      code: ""
    });
    //   表单的验证
    const rules = reactive({
      pass: [{ validator: formpass, trigger: "blur" }],
      repass: [{ validator: formrepass, trigger: "blur" }],
      code: [{ validator: formcode, trigger: "blur" }],
      mail: [{ validator: formmail, trigger: "blur" }]
    });
    //   console.log(isRef(menuTab)? '是基础数据类型' : '是对象类型');
    /*     const obj = reactive({
        x: 0,
        y: 1
    })
    const aa = toRefs(obj);
    console.log(obj.y);
     */
    /*声明函数*/

    // vue 数据驱动视图渲染 ， 而原生js 操作DOM元素
    const toggleMenu = data => {
      menuTab.forEach((elem, index) => {
        elem.current = false;
      });
      // 高光
      data.current = true;
      //   修改模块值
      model.value = data.type;
    };
    const signForm = formName => {
      refs[formName].validate(valid => {
        if (valid) {
          alert("sign in");
        } else {
          console.log("error sign in");
          return false;
        }
      });
    };
    /**
     * 生命周期
     * 挂载完成后  */

    onMounted(() => {});
    return {
      menuTab,
      model,
      ruleForm,
      rules,
      toggleMenu,
      signForm
    };
  },

  created() {},
  // 这里是挂载完成后自动执行的
  mounted() {}
  // 写函数的地方
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

