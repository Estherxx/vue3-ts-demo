<template>
  <div class="login-box">
    <el-form
      ref="ruleFormRef"
      :model="ruleForm"
      status-icon
      :rules="rules"
      label-width="60px"
      class="demo-ruleForm"
    >
      <h2>后台管理系统</h2>
      <el-form-item label="账号" prop="username">
        <el-input v-model="ruleForm.username" autocomplete="off" />
      </el-form-item>
      <el-form-item label="密码" prop="password">
        <el-input
          v-model="ruleForm.password"
          type="password"
          autocomplete="off"
        />
      </el-form-item>
      <el-form-item>
        <el-button
          class="loginbtn"
          type="primary"
          @click="submitForm(ruleFormRef)"
          >Submit</el-button
        >
        <el-button class="loginbtn" @click="resetForm(ruleFormRef)"
          >Reset</el-button
        >
      </el-form-item>
    </el-form>
  </div>
</template>

<script lang="ts">
import { defineComponent, reactive, toRefs, ref } from "vue";
import { LoginData } from "../type/login";
import type { FormInstance } from "element-plus";
import { login } from "../request/api";
import { useRouter } from "vue-router";
export default defineComponent({
  setup() {
    const data = reactive(new LoginData());
    const rules = {
      username: [
        {
          required: true,
          message: "Please input Activity name",
          trigger: "blur",
        },
        {
          min: 3,
          max: 10,
          message: "Length should be 3 to 10",
          trigger: "blur",
        },
      ],
      password: [
        {
          required: true,
          message: "Please input password",
          trigger: "blur",
        },
        {
          min: 3,
          max: 10,
          message: "Length should be 3 to 10",
          trigger: "blur",
        },
      ],
    };
    //登录
    const ruleFormRef = ref<FormInstance>();
    const router = useRouter();
    const submitForm = (formEl: FormInstance | undefined) => {
      if (!formEl) return;
      formEl.validate((valid) => {
        if (valid) {
          console.log("submit!");
          login(data.ruleForm).then((res) => {
            console.log(res);
            //将token保存
            localStorage.setItem("token", res.data.token);
            //跳转页面
            router.push("/");
          });
        } else {
          console.log("error submit!");
          return false;
        }
      });
    };
    //重置
    const resetForm = () => {
      data.ruleForm.username = "";
      data.ruleForm.password = "";
    };
    return { ...toRefs(data), rules, resetForm, ruleFormRef, submitForm };
  },
});
</script>

<style lang="scss" scoped>
.login-box {
  width: 100%;
  height: 100%;
  padding: 1px;
  background-color: rgb(243, 229, 233);
  text-align: center;
  .demo-ruleForm {
    width: 500px;
    margin: 200px auto;
    background-color: lightblue;
    padding: 30px;
    border-radius: 20px;
  }
  .loginbtn {
    width: 48%;
  }
  h2 {
    margin-bottom: 20px;
  }
}
</style>
