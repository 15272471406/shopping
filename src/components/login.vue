<template>
  <div class="login">
    <div class="login-con">
      <h1>欢迎登录</h1>
      <Form ref="formInline" :model="formInline" :rules="ruleInline">
        <FormItem prop="user">
            <Input type="text" v-model="formInline.user" placeholder="用户名">
                <Icon type="ios-person-outline" slot="prepend"></Icon>
            </Input>
        </FormItem>
        <FormItem prop="password">
            <Input type="password" v-model="formInline.password" placeholder="密码">
                <Icon type="ios-lock-outline" slot="prepend"></Icon>
            </Input>
        </FormItem>
        <FormItem>
            <Button type="primary" @click="handleSubmit('formInline')">登录</Button>
            <Button type="primary" @click="handleSubmit('formInline')">忘记密码</Button>
        </FormItem>
    </Form>
    </div>
  </div>
</template>

<script>
export default {
  name: 'login',
  data () {
    return {
      formInline: {
        user: '',
        password: ''
      },
      ruleInline: {
        user: [
          { required: true, message: '请输入用户名', trigger: 'blur' }
        ],
        password: [
          { required: true, message: '请输入密码', trigger: 'blur' },
          { type: 'string', min: 6, message: '密码最小长度为6', trigger: 'blur' }
        ]
      }
    }
  },
  mounted () {
    
  },
  methods: {
    handleSubmit(name) {
      this.$refs[name].validate((valid) => {
          if (valid) {
            depot.post({
              url: 'login',
              data: this.formInline,
              cb: (res)=> {
                let {code} = res.data
                if (code === 9) {
                  this.$Message.error('账号或密码错误')
                } else {
                  this.$Message.success('登录成功')
                }
              }
            })
          } else {
              this.$Message.error('请输入账号密码')
          }
      })
    }
  }
}
</script>

<style scoped lang="less">
.login{
  width: 100%;
  height: 100vh;
  background: url("../assets/images/login.jpg") no-repeat;
  background-size: cover;
  .login-con {
    position: absolute;
    right: 160px;
    top: 50%;
    -webkit-transform: translateY(-60%);
    transform: translateY(-60%);
    width: 300px;
    background: #ffffff;
    border-radius: 6px;
  }
  h1{
    padding: 10px;
    font-size: 20px;
    border-bottom: 1px solid #eeddee;
  }
  .ivu-form{
    padding: 10px;
  }
  .ivu-btn-primary{
    width: 100%;
    margin-bottom: 10px;
  }
}

</style>
