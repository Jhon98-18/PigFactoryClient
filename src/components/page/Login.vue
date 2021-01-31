<template>
    <div class="login-wrap">
        <div class="ms-login">
            <div class="ms-title">猪场后台管理系统</div>
            <el-form :model="param" :rules="rules" ref="login" label-width="0px" class="ms-content">
                <el-form-item prop="userName">
                    <!--                    props 规则验证的字段名称-->
                    <el-input v-model="param.userName" placeholder="userName">
                        <!--                        v-model是绑定字段的   placeholder  默认提示字-->
                        <el-button slot="prepend" icon="el-icon-lx-people"></el-button>
                    </el-input>
                </el-form-item>

                <el-form-item prop="passWord">
                    <el-input
                            type="password"
                            placeholder="passWord"
                            v-model="param.passWord"
                            @keyup.enter.native="submitForm()"
                    >
                        <el-button slot="prepend" icon="el-icon-lx-lock"></el-button>
                    </el-input>
                </el-form-item>
                <div class="login-btn">
                    <el-button type="primary" @click="submitForm()">登录</el-button>
                </div>
                <p class="login-tips">Tips : 你好欢迎光临长华养殖合作社。</p>
            </el-form>
        </div>
    </div>
</template>

<script>
    export default {
        data: function () {
            return {
                param: {
                    userName: '',
                    passWord: '',
                },
                rules: {
                    userName: [{required: true, message: '请输入用户名', trigger: 'blur'}],
                    passWord: [{required: true, message: '请输入密码', trigger: 'blur'}],
                }
            };
        },
        methods: {
            submitForm: function () {
                this.$refs.login.validate(valid => {
                    if (valid) {
                        this.$axios.post("http://localhost:8888/auth/login", {
                            'userName': this.param.userName,
                            'passWord': this.param.passWord
                        }).then((response) => {
                            var token = response.headers.token;
                            if (token == null) {
                                this.$message.error('账号密码有误');
                            } else {
                                localStorage.setItem('ms_username', this.param.userName);
                                localStorage.setItem('token', response.headers.token);
                                this.$router.push('/table');
                            }
                        }, error => {
                            this.$message.error('错误');
                            this.$router.push('/login');
                        })
                        // this.$axios({
                        //     headers:
                        //         {
                        //             'Content-Type': 'application/json'
                        //         },
                        //     url: 'http://localhost:8888/login',
                        //     method: 'get',
                        //     data: {
                        //         'userName': 'Daniel',
                        //         'passWord': 'Daniel',
                        //     }
                        // }).then((response) => {
                        //     alert("登录成功")
                        // }).catch(function (error) {
                        //     alert("登录失败")
                        // });
                    } else {
                        this.$message.error('请输入账号和密码');
                        console.log('error submit!!');
                        return false;
                    }
                });
            },
        },
    };
</script>

<style scoped>
    .login-wrap {
        position: relative;
        width: 100%;
        height: 100%;
        background-image: url(../../assets/img/login-bg.jpg);
        background-size: 100%;
    }

    .ms-title {
        width: 100%;
        line-height: 50px;
        text-align: center;
        font-size: 20px;
        color: #fff;
        border-bottom: 1px solid #ddd;
    }

    .ms-login {
        position: absolute;
        left: 50%;
        top: 50%;
        width: 350px;
        margin: -190px 0 0 -175px;
        border-radius: 5px;
        background: rgba(255, 255, 255, 0.3);
        overflow: hidden;
    }

    .ms-content {
        padding: 30px 30px;
    }

    .login-btn {
        text-align: center;
    }

    .login-btn button {
        width: 100%;
        height: 36px;
        margin-bottom: 10px;
    }

    .login-tips {
        font-size: 12px;
        line-height: 30px;
        color: #fff;
    }
</style>
