<template>
    <div style="width: 100%; height: 100vh;background-color: bisque; overflow: hidden">
        <div style="width: 400px;margin: 150px auto">
            <div style="color: darkblue; font-size: 30px;text-align: center;padding: 30px 0">欢迎登录</div>
            <el-form
                    :model="form"
                    :rules="rules"
                    ref="form"
                    class="demo-ruleForm"
                    size="normal"
                    status-icon
            >
                <el-form-item prop="username">
                    <el-input v-model="form.username">
                        <el-icon class="el-icon--right">
                            <UserFilled />// 添加一个代表用户的图标
                        </el-icon>
                    </el-input>
                </el-form-item>
                <el-form-item prop="password" >
                    <el-input v-model="form.password" show-password>
                        <el-icon class="el-icon--right"><Lock /></el-icon>
                    </el-input>
                </el-form-item>
                <el-form-item>
                    <el-button style="width: 100%" type="primary" @click="login">登录</el-button>
                </el-form-item>
            </el-form>
        </div>
    </div>
</template>

<script>
    import request from "../utils/request";
    import { Avatar,Lock } from '@element-plus/icons-vue'
    export default {
        name: "Login",
        data(){
            return{
                form:{},
                rules:{
                    username:[
                        {required:true,message:'请输入用户名',trigger:'blur'}
                    ],
                    password:[
                        {required:true,message:'请输入密码',trigger:'blur'}
                    ]
                }
            }
        },
        components:{
            Avatar,Lock
        },
        methods:{
            login(){
                this.$refs["form"].validate((valid) => {
                    if(valid){
                        request.post("/user/login",this.form).then(res => {
                            if(res.code == '0'){
                                this.$message({
                                    type:"success",
                                    message:"登录成功"
                                })
                                this.$router.push("/")
                            }else {
                                this.$message({
                                    type:"error",
                                    message:res.msg
                                })
                            }
                        })
                    }
                })

            }
        }
    }
</script>

<style scoped>

</style>