<template>
    <div class="login_container">
        <div class="login_box">
            <div class="logo_icon">
                <img src="../assets/logo.png" alt="">
            </div>
            <!-- 用户名 -->
            <el-form ref="loginFormRef" :model="loginForm" :rules="rules" label-width="0px" class="login_form">
                <el-form-item prop="username">
                    <el-input v-model="loginForm.username" prefix-icon="iconfont icon-user"></el-input>
                </el-form-item>
            <!-- 密码 -->
                <el-form-item prop="password">
                    <el-input v-model="loginForm.password" prefix-icon="iconfont icon-3702mima" type="password"></el-input>
                </el-form-item>
            <!-- 按钮 -->
                <el-form-item class='btns'>
                    <el-button type="primary" @click="validateForm">登录</el-button>
                    <el-button type="info" @click="resetLoginForm">重置</el-button>
                </el-form-item>
            </el-form>
        </div>
    </div>
</template>

<script>
import { async } from 'q';
export default {
    data(){
        return{
            loginForm:{
                username:'admin',
                password:'123456'
            },
            // 验证规则
            rules: {
                username: [
                  { required: true, message: '请输入用户名', trigger: 'blur' },
                  { min: 3, max: 5, message: '长度在 3 到 8 个字符', trigger: 'blur' }
                ],
                password: [
                  { required: true, message: '请输入密码', trigger: 'change' },
                  { min: 5, max: 12, message: '长度在 6 到 12 个字符', trigger: 'blur' }

                ]
            }
        }
    },
    methods: {
        // 重置事件处理函数
        resetLoginForm(){
            this.$refs.loginFormRef.resetFields();
        },
        validateForm(){
            this.$refs.loginFormRef.validate(async val=>{
                if(!val) return;
                const {data:res} = await this.$http.post('login', this.loginForm);
                if(res.meta.status != 200) return this.$message.error('登陆失败')
                this.$message.success('登陆成功')
                window.sessionStorage.setItem('token',res.data.token)
                this.$router.push('/home')
            })
        }
    }
    
}
</script>

<style lang="less" scoped>
.login_container{
    height: 100%;
    background-color: #2b4b6b;
}
.login_box{
    width: 450px;
    height: 300px;
    background-color: #fff;
    border-radius: 3px; 
    position: absolute;
    left: 50%;
    top:50%;
    transform: translate(-50%,-50%);
    .logo_icon{
        width: 130px;
        height: 130px;
        background-color: #fff;
        border-radius: 50%;
        box-shadow: 0 0 10px #ddd;
        border: 1px solid #eee;
        padding: 10px;
        position: absolute;
        left: 50%;
        transform: translate(-50%,-50%);

        img{
            width: 100%;
            height: 100%;
            border-radius:50%; 
            background-color: #eee;
        }
    }
}

.btns{
    display: flex;
    justify-content: flex-end;
}
.login_form{
    box-sizing: border-box;
    width: 100%;
    padding: 20px;
    position:absolute;
    bottom: 0;
}
</style>