<template>
  <div class="bg">
    <div style="width: 350px; background-color: #fff; border-radius: 5px; box-shadow: 0 0 10px rgba(0,0,0,0.1); padding: 40px 20px">
      <el-form ref="formRef" :model="data.form" :rules="data.rules">
        <div style="margin-bottom: 40px; text-align: center; font-weight: bold; font-size: 24px">欢 迎 登 录</div>
        <el-form-item prop="username">
          <el-input size="large" v-model="data.form.username" autocomplete="off" prefix-icon="User" placeholder="请输入账号" />
        </el-form-item>
        <el-form-item prop="password">
          <el-input size="large" show-password v-model="data.form.password" autocomplete="off" prefix-icon="Lock" placeholder="请输入密码" />
        </el-form-item>
        <el-form-item prop="role">
          <el-select size="large" style="width: 100%" v-model="data.form.role">
            <el-option label="管理员" value="ADMIN"></el-option>
            <el-option label="普通用户" value="USER"></el-option>
          </el-select>
        </el-form-item>
        <div style="margin-bottom: 20px">
          <el-button style="width: 100%" size="large" type="primary" @click="login">登 录</el-button>
        </div>
        <div style="text-align: right">
          还没有账号？请 <a style="color: #274afa" href="/register">注册</a>
        </div>
      </el-form>
    </div>
<!--    <el-dialog title="郑重声明" v-model="data.dialogVisible" :show-close="false" width="40%" :close-on-click-modal="false" destroy-on-close>-->
<!--      <div style="font-size: 16px; line-height: 26px; margin-bottom: 20px; text-align: justify">-->
<!--        本平台已经对本项目申请了<b style="color: #000">软件著作权</b>，完善了所有该项目资源相关的法律文件材料。-->
<!--        <b style="color: #ff2424">本平台付费用户可以学习该项目自己使用，禁止在其他平台做转手或者倒卖，禁止在私域未经授权分享源码，禁止上传代码到github、gitee、gitlab等代码托管平台。</b>-->
<!--        我们公司的法务会在各大平台（例如：闲鱼、小红书、B站、CSDN等社交媒体平台）不定期检索， 一旦发现倒卖或转手的现象，我们会第一时间收集好证据，向你发送律师函。-->
<!--        <b style="color: #000">现在互联网所有账号都是实名制，我们可以明确追溯到你。请大家不要做违法的事情，不要因为一时糊涂给自己的人生轨迹抹上一层黑。</b>-->
<!--      </div>-->
<!--      <div style="margin-top: 10px; font-size: 16px; color: #000">-->
<!--        本项目唯一官方平台：<b>项目实战网（<a href="https://www.javaxmsz.cn" target="_blank">https://www.javaxmsz.cn</a>）</b>-->
<!--      </div>-->
<!--      <div style="margin-top: 5px; font-size: 16px"></div>-->
<!--      <template #footer>-->
<!--        <span class="dialog-footer">-->
<!--          <el-button type="primary" @click="data.dialogVisible=false">我已明确法律规则，承诺本项目仅限个人使用</el-button>-->
<!--        </span>-->
<!--      </template>-->
<!--    </el-dialog>-->
  </div>
</template>

<script setup>
import { reactive, ref } from "vue";
import request from "@/utils/request.js";
import {ElMessage} from "element-plus";
import router from "@/router/index.js";

const formRef = ref()
const data = reactive({
  dialogVisible: true,
  form: { role: 'ADMIN' },
  rules: {
    username: [
      { required: true, message: '请输入账号', trigger: 'blur' },
      { min: 3, message: '账号最少3位', trigger: 'blur' },
    ],
    password: [
      { required: true, message: '请输入密码', trigger: 'blur' }
    ],
  }
})

const login = () => {
  formRef.value.validate((valid) => {
    if (valid) {
      request.post('/login', data.form).then(res => {
        if (res.code === '200') {
          // 存储用户信息
          localStorage.setItem("code_user", JSON.stringify(res.data || {}))
          ElMessage.success('登录成功')
          // router.push('/')
          if ("USER" === res.data.role) {
            location.href = '/front/home'
          } else {
            router.push('/')
          }
        } else {
          ElMessage.error(res.msg)
        }
      })
    }
  })
}
</script>

<style scoped>
.bg {
  height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  overflow: hidden;
  background-image: url("@/assets/imgs/bg.jpg");
  background-size: cover;
}
</style>