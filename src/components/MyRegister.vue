<!--
 * @Description: 用户注册组件
 * @Author: hai-27
 * @Date: 2020-02-19 22:20:35
 * @LastEditors: hai-27
 * @LastEditTime: 2020-03-01 15:34:34
 -->
<template>
  <div id="register">
    <el-dialog title="注册" width="400px" center :visible.sync="isRegister">
      <el-form
        :model="RegisterUser"
        :rules="rules"
        status-icon
        ref="ruleForm"
        class="demo-ruleForm"
      >
        <el-form-item label= "头像：">
          <el-upload
            class="avatar-uploader"
            action="/api/upload"
            :show-file-list="false"
            :on-success="handleAvatarSuccess"
            :before-upload="beforeAvatarUpload">
              <img v-if="RegisterUser.photo" :src="RegisterUser.photo"  class="avatar">
              <i v-else class="el-icon-plus avatar-uploader-icon"></i>
        </el-upload>
        </el-form-item>
        <el-form-item prop="name">
          <el-input
            prefix-icon="el-icon-user-solid"
            placeholder="请输入用户名"
            v-model="RegisterUser.name"
          ></el-input>
        </el-form-item>
        <el-form-item prop="account">
          <el-input
            prefix-icon="el-icon-user-solid"
            placeholder="请输入账号"
            v-model="RegisterUser.account"
          ></el-input>
        </el-form-item>
        <el-form-item prop="pass">
          <el-input
            prefix-icon="el-icon-view"
            type="password"
            placeholder="请输入密码"
            v-model="RegisterUser.pass"
          ></el-input>
        </el-form-item>
        <el-form-item prop="confirmPass">
          <el-input
            prefix-icon="el-icon-view"
            type="password"
            placeholder="请再次输入密码"
            v-model="RegisterUser.confirmPass"
          ></el-input>
        </el-form-item>
        <el-form-item label="性别：" prop="sex">
          <el-radio-group v-model="RegisterUser.sex">
            <el-radio label="保密"></el-radio>
            <el-radio label="男"></el-radio>
            <el-radio label="女"></el-radio>
          </el-radio-group>
        </el-form-item>
        <el-form-item prop="email">
          <el-input
            prefix-icon="el-icon-user-solid"
            placeholder="请输入邮箱"
            v-model="RegisterUser.email"
          ></el-input>
        </el-form-item>
        <el-form-item prop="phone">
          <el-input
            prefix-icon="el-icon-user-solid"
            placeholder="请输入手机号"
            v-model="RegisterUser.phone"
          ></el-input>
        </el-form-item>
        <el-form-item>
          <el-button size="medium" type="primary" @click="Register" style="width:100%;">注册</el-button>
        </el-form-item>
      </el-form>
    </el-dialog>
  </div>
</template>
<script>
export default {
  name: "MyRegister",
  props: ["register"],
  data() {
    // 用户名的校验方法
    let validateName  = (rule, value, callback) => {
      if (value === "") {
        return callback(new Error("请输入用户名"));
      }
    };
    // 账号的校验方法
    let validateAccount = (rule, value, callback) => {
      if (!value) {
        return callback(new Error("请输入账号"));
      }
      // 用户名以字母开头,长度在5-16之间,允许字母数字下划线
      // const userNameRule = /^[a-zA-Z][a-zA-Z0-9_]{4,15}$/;
      // if (userNameRule.test(value)) {
      //   //判断数据库中是否已经存在该用户名
      //   this.$axios
      //     .post("/api/users/findUserName", {
      //       userName: this.RegisterUser.name
      //     })
      //     .then(res => {
      //       // “001”代表用户名不存在，可以注册
      //       if (res.data.code == "001") {
      //         this.$refs.ruleForm.validateField("checkPass");
      //         return callback();
      //       } else {
      //         return callback(new Error(res.data.msg));
      //       }
      //     })
      //     .catch(err => {
      //       return Promise.reject(err);
      //     });
      // } else {
      //   return callback(new Error("字母开头,长度5-16之间,允许字母数字下划线"));
      // }
    };
    // 密码的校验方法
    let validatePass = (rule, value, callback) => {
      if (value === "") {
        return callback(new Error("请输入密码"));
      }
    };
    // 确认密码的校验方法
    let validateConfirmPass = (rule, value, callback) => {
      if (value === "") {
        return callback(new Error("请输入确认密码"));
      }
      // 校验是否以密码一致
      if (this.RegisterUser.pass != "" && value === this.RegisterUser.pass) {
        this.$refs.ruleForm.validateField("checkPass");
        return callback();
      } else {
        return callback(new Error("两次输入的密码不一致"));
      }
    };
    // 邮箱的校验方法
    let validateEmail  = (rule, value, callback) => {
    if (value === "") {
      return callback(new Error("请输入邮箱"));
    }
    };
    // 手机号的校验方法
    let validatePhone  = (rule, value, callback) => {
    if (value === "") {
      return callback(new Error("请输入手机号"));
    }
    };
    return {
      isRegister: false, // 控制注册组件是否显示
      RegisterUser: {
        name: "",
        account:"",
        pass: "",
        confirmPass: "",
        sex:"",
        email:"",
        phone:"",
        photo:""
      },
      // 用户信息校验规则,validator(校验方法),trigger(触发方式),blur为在组件 Input 失去焦点时触发
      rules: {
        name: [{ validator: validateName, trigger: "blur" }],
        account: [{ validator: validateAccount, trigger: "blur" }],
        pass: [{ validator: validatePass, trigger: "blur" }],
        confirmPass: [{ validator: validateConfirmPass, trigger: "blur" }],
        email: [{ validator: validateEmail, trigger: "blur" }],
        phone: [{ validator: validatePhone, trigger: "blur" }]
      }
    };
  },
  watch: {
    // 监听父组件传过来的register变量，设置this.isRegister的值
    register: function(val) {
      if (val) {
        this.isRegister = val;
      }
    },
    // 监听this.isRegister变量的值，更新父组件register变量的值
    isRegister: function(val) {
      if (!val) {
        this.$refs["ruleForm"].resetFields();
        this.$emit("fromChild", val);
      }
    }
  },
  methods: {
    Register() {
      // 通过element自定义表单校验规则，校验用户输入的用户信息
      this.$refs["ruleForm"].validate(valid => {
        //如果通过校验开始注册
        if (valid) {
          this.$axios
            .post("/api/users/register", {
              userName: this.RegisterUser.name,
              password: this.RegisterUser.pass
            })
            .then(res => {
              // “001”代表注册成功，其他的均为失败
              if (res.data.code === "001") {
                // 隐藏注册组件
                this.isRegister = false;
                // 弹出通知框提示注册成功信息
                this.notifySucceed(res.data.msg);
              } else {
                // 弹出通知框提示注册失败信息
                this.notifyError(res.data.msg);
              }
            })
            .catch(err => {
              return Promise.reject(err);
            });
        } else {
          return false;
        }
      });
    },
    beforeAvatarUpload(file) {
       const isLt2M = file.size / 1024 / 1024 < 2;
       if (!isLt2M) {
          this.$message.error('上传头像图片大小不能超过 2MB!');
         }
        return isLt2M;
   },
   handleAvatarSuccess(res) {
      this.RegisterUser.photo = res; // 假设后端返回的是图片的访问路径
  },
  }
};
</script>
<style>
@import url("//unpkg.com/element-ui@2.15.14/lib/theme-chalk/index.css");
    .avatar-uploader .el-upload {
        border: 1px dashed #d9d9d9;
        border-radius: 6px;
        cursor: pointer;
        position: relative;
        overflow: hidden;
    }
    .avatar-uploader .el-upload:hover {
        border-color: #409EFF;
    }
    .avatar-uploader-icon {
        font-size: 28px;
        color: #8c939d;
        width: 178px;
        height: 150px;
        line-height: 150px;
        text-align: center;
    }
    .avatar {
        width: 178px;
        height: 150px;
        display: block;
    }
</style>