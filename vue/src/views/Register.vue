<template>
    <div style="width: 100%; height: 100vh;background-color: bisque; overflow: hidden">
        <div style="width: 400px;margin: 150px auto">
            <div style="color: darkblue; font-size: 30px;text-align: center;padding: 30px 0">欢迎注册</div>
            <el-form
                    ref="ruleFormRef"
                    :model="ruleForm"
                    :rules="rules"
                    class="demo-ruleForm"
                    size="normal"
                    status-icon
            >
                <el-form-item label="Username:" prop="username">
                    <el-input v-model="ruleForm.username" >
                        <el-icon><User /></el-icon>
                    </el-input>
                </el-form-item>
                <el-form-item label="Password:" prop="password">
                    <el-input v-model="ruleForm.password" show-password >
                        <el-icon><Lock /></el-icon>
                    </el-input>
                </el-form-item>
                <el-form-item label="Confirm:" prop="confirm">
                    <el-input v-model="ruleForm.confirm" show-password >
                        <el-icon><Lock /></el-icon>
                    </el-input>
                </el-form-item>
                <el-form-item>
                    <el-button style="width: 100%" type="primary" @click="register">注册</el-button>
                </el-form-item>
            </el-form>
        </div>
    </div>
</template>

<script>
    import request from "../utils/request";


    export default {
        name: "Register",
        data(){
            return{
                ruleForm:{},
                rules:{
                    username:[
                        {required:true,message:'请输入用户名',trigger:'blur'}
                    ],
                    password:[
                        {required:true,message:'请输入密码',trigger:'blur'}
                    ],
                    confirm:[
                        {required:true,message:'请确认密码',trigger:'blur'}
                    ]
                }
            }
        },
        components:{

        },
        methods:{
            register(){
                if (this.ruleForm.password != this.ruleForm.confirm){
                    this.$message({
                        type:"error",
                        message:"两次输入的密码不一致!!"
                    })
                    return
                }
                this.$refs['ruleFormRef'].validate((valid) => {
                    if(valid){
                        request.post("/user/register",this.ruleForm).then(res => {
                            if(res.code == '0'){
                                this.$message({
                                    type:"success",
                                    message:"注册成功"
                                })
                                this.$router.push("/login")
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