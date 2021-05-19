<template>
    <div class="login_container">
        <!--登录块-->
        <div class="login_box">
            <!--logo-->
            <div class="avator_box">
                <img src="../assets/logo.png" alt=""/>
            </div>
            <!--表单区域-->
            <el-form ref="loginFormRef" :model="loginForm" :rules="loginRules" class="login_form" label-width="0">
                <!--用户名-->
                <el-form-item prop="username">
                    <el-input v-model="loginForm.username" prefix-icon="iconfont icondenglu"></el-input>
                </el-form-item>
                <!--密码-->
                <el-form-item prop="password">
                    <el-input v-model="loginForm.password" prefix-icon="iconfont iconmima" type="password"></el-input>
                </el-form-item>
                <!--按钮-->
                <el-form-item class="btns">
                    <el-button type="primary" @click="login">提交</el-button>
                    <el-button type="info" @click="resetLoginForm">重置</el-button>
                </el-form-item>
            </el-form>
        </div>
    </div>
</template>

<script>
    export default {
        data() {
            return {
                //表单数据对象
                loginForm: {
                    username: "admin",
                    password: "123456"
                },
                //验证对象
                loginRules: {
                    //校验用户名
                    username: [
                        {required: true, message: "请输入用户名", trigger: "blur"},//必填项验证
                        {min: 5, max: 8, message: "长度在 5 到 8 个字符", trigger: "blur"}//长度验证
                    ],
                    //校验密码
                    password: [
                        {required: true, message: "请输入密码", trigger: "blur"},//必填项验证
                        {min: 6, max: 8, message: "密码为 6~8 位", trigger: "blur"}//长度验证
                    ],
                },
            };
        },
        methods: {
            resetLoginForm() {
                this.$refs.loginFormRef.resetFields();
            },
            login() {
                this.$refs.loginFormRef.validate(async valid => {
                    if (!valid) return;
                    // 调用get请求
                    //const {data: res} = await this.$http.post("test"); //访问后台
                    //console.log(res)
                    const {data: res} = await this.$http.post("login", this.loginForm);
                    if (res.flag== "OK") {
                        window.sessionStorage.setItem("user",res.user); // 存储user对象
                        this.$message.success("登陆成功！！！");  //信息提示
                        await this.$router.push({path: "/home"});   //页面路由跳转
                    } else {
                        this.$message.error("登录失败！！！");   //错误提示
                    }
                });
            }
        },

    }
</script>

<style lang="less" scoped>
    .login_container {
        background-color: #2b4b6b;
        height: 100%;
    }

    .login_box {
        width: 450px;
        height: 300px;
        background-color: #fff;
        border-radius: 5px;
        position: absolute;
        left: 50%;
        top: 45%;
        transform: translate(-50%, -50%);

        .avator_box {
            width: 130px;
            height: 130px;
            border: 1px solid #eee;
            border-radius: 50%;
            padding: 1px;
            position: absolute;
            left: 50%;
            transform: translate(-50%, -50%);
            background-color: #0ee;

            img {
                width: 100%;
                height: 100%;
                border-radius: 50%; //圆角
                background-color: #eee;
            }
        }

        .login_form {
            position: absolute;
            bottom: 0%;
            width: 100%;
            padding: 0 10px;
            box-sizing: border-box; // 设置边框
        }

        .btns {
            display: flex; // 弹性布局
            justify-content: center; // 尾部对齐

        }

    }
</style>