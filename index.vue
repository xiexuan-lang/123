<template>
    <div class="w-box index">

        <v-title>{{$L('谢轩协作平台')}}</v-title>

        <div class="welbg">
            <div class="second">
                <div class="bg"></div>
            </div>
        </div>

        <div class="header">
            <div class="z-row">
                <div class="header-col-sub">
                    <h2>
                        <img v-if="systemConfig.logo" :src="systemConfig.logo">
                        <img v-else src="../../../statics/images/logo-white.png">
                        <span>{{$L('谢轩协作平台')}}</span>
                    </h2>
                </div>
                <div class="z-1">
                    <dl>
                        <dd>
                            <Button v-if="systemConfig.enterprise=='show'" type="success" size="small" class="right-enterprise" @click="enterpriseOpen">{{$L('找xiexuan')}}</Button>
                            <a v-if="systemConfig.github=='show'" href='https://gitee.com/aipaw/wookteam' class="right-info" target="_blank">
                                <img class="right-img" src="https://gitee.com/aipaw/wookteam/badge/star.svg?theme=gvp" alt="star"/>
                            </a>
                            <a v-if="systemConfig.github=='show'" class="right-info" target="_blank" href="https://gitee.com/aipaw/wookteam">
                                <Icon class="right-icon" type="logo-github"/>
                            </a>
                            <Dropdown class="right-info" trigger="hover" @on-click="setLanguage" transfer>
                                <div>
                                    <Icon class="right-icon" type="md-globe"/>
                                    <Icon type="md-arrow-dropdown"/>
                                </div>
                                <Dropdown-menu slot="list">
                                    <Dropdown-item name="zh" :selected="getLanguage() === 'zh'">中文</Dropdown-item>
                                    <Dropdown-item name="en" :selected="getLanguage() === 'en'">English</Dropdown-item>
                                </Dropdown-menu>
                            </Dropdown>
                        </dd>
                    </dl>
                </div>
            </div>
        </div>

        <div class="welcome">
            <div class="banner">
                <div class="z-row">
                    <div class="z-16">
                        <Carousel class="banner-carousel" autoplay loop :autoplay-speed="5000">
                            <CarouselItem>
                                <img src="../../../statics/images/index/banner/1.jpg">
                            </CarouselItem>
                            <CarouselItem>
                                <img src="../../../statics/images/index/banner/2.jpg">
                            </CarouselItem>
                        </Carousel>
                    </div>
                    <div class="z-8"><h3>{{$L('xiexuan开发的协作平台')}}</h3>
                        <div class="bl inline-block">
                            <span class="start" @click="loginChack">{{$L('立即登陆')}}</span>
                        </div>
                    </div>
                </div>
            </div>
            <div class="second">
                <div class="bg"></div>
                <div class="z-row">
                    <div class="z-6"><a href="#W_link1"><i class="ft icon">&#xe753;</i>{{$L('待办四象限')}}</a></div>
                    <div class="z-6"><a href="#W_link2"><i class="ft icon">&#xe6b8;</i>{{$L('项目管理')}}</a></div>
                    <div class="z-6"><a href="#W_link3"><i class="ft icon">&#xe915;</i>{{$L('在线知识库')}}</a></div>
                    <div class="z-6"><a href="#W_link4"><i class="ft icon">&#xe706;</i>{{$L('日程管理')}}</a></div>
                </div>
            </div>
        </div>

        <div class="p-footer"><span v-html="systemConfig.footerText || 'xiexuan &copy; 2018-2022'"></span></div>

        <Modal
            v-model="loginShow"
            :mask-closable="false"
            class-name="simple-modal">
            <Form ref="login" :model="formLogin" :rules="ruleLogin" @submit.native.prevent>
                <FormItem prop="username">
                    <Input type="text" v-model="formLogin.username" :placeholder="$L('用户名')" @on-enter="onLogin" @on-blur="onBlur">
                        <Icon type="ios-person-outline" slot="prepend"></Icon>
                    </Input>
                </FormItem>
                <FormItem prop="userpass">
                    <Input type="password" v-model="formLogin.userpass" :placeholder="$L('密码')" @on-enter="onLogin">
                        <Icon type="ios-lock-outline" slot="prepend"></Icon>
                    </Input>
                </FormItem>
                <FormItem v-if="loginType=='reg'" prop="userpass2">
                    <Input type="password" v-model="formLogin.userpass2" :placeholder="$L('确认密码')" @on-enter="onLogin">
                        <Icon type="ios-lock-outline" slot="prepend"></Icon>
                    </Input>
                </FormItem>
                <FormItem v-if="loginType=='login'&&codeNeed" prop="code">
                    <Input type="text" v-model="formLogin.code" :placeholder="$L('验证码')" @on-enter="onLogin">
                        <Icon type="ios-checkmark-circle-outline" slot="prepend"></Icon>
                        <div slot="append" class="login-code" @click="refreshCode"><img :src="codeUrl"/></div>
                    </Input>
                </FormItem>
            </Form>
            <div slot="header" class="login-header">
                <div @click="loginType='login'" class="login-header-item" :class="{active:loginType=='login'}">{{$L('用户登录')}}</div>
                <div v-if="systemConfig.reg=='open'" @click="loginType='reg'" class="login-header-item" :class="{active:loginType=='reg'}">{{$L('注册账号')}}</div>
            </div>
            <div slot="footer">
                <Button type="default" @click="loginShow=false">{{$L('取消')}}</Button>
                <Button type="primary" :loading="loadIng > 0" @click="onLogin">{{$L(loginType=='reg'?'注册':'登录')}}</Button>
            </div>
        </Modal>
    </div>
</template>

<style lang="scss">
.login-header {
    display: flex;
    align-items: center;
    .login-header-item {
        height: 20px;
        line-height: 20px;
        font-size: 14px;
        color: #444444;
        overflow: hidden;
        text-overflow: ellipsis;
        white-space: nowrap;
        padding-right: 12px;
        cursor: pointer;
        &.active {
            font-size: 16px;
            color: #17233d;
            font-weight: 500;
        }
    }
}
.login-code {
    margin: -4px -7px;
    height: 30px;
    overflow: hidden;
    cursor: pointer;
    img {
        height: 100%;
    }
}
</style>
<style lang="scss" scoped>
.index {
    position: absolute;
    color: #000000;
    top: 0;
    left: 0;
    min-width: 100%;
    min-height: 100%;
    padding: 0;
    margin: 0;
    .header {
        position: relative;
        z-index: 3;
        height: 50px;
        padding-top: 12px;
        max-width: 1280px;
        margin: 0 auto;
        .z-row {
            color: #fff;
            height: 50px;
            position: relative;
            z-index: 2;
            max-width: 1680px;
            margin: 0 auto;
            .header-col-sub {
                width: 500px;
                h2 {
                    position: relative;
                    padding: 1rem 0 0 1rem;
                    display: flex;
                    align-items: flex-end;
                    img {
                        width: 150px;
                        margin-right: 6px;
                    }
                    span {
                        font-size: 12px;
                        font-weight: normal;
                        color: rgba(255, 255, 255, 0.85);
                        line-height: 14px;
                    }
                }
            }
            .z-1 {
                dl {
                    position: absolute;
                    right: 20px;
                    top: 0;
                    font-size: 14px;
                    dd {
                        line-height: 50px;
                        color: #fff;
                        cursor: pointer;
                        margin-right: 1px;
                        .right-enterprise {
                            padding: 1px 10px;
                            font-size: 12px;
                            color: #f6ca9d;
                            background: #1d1e23;
                            background: linear-gradient(90deg, #1d1e23, #3f4045);
                            border: none;
                        }
                        .right-info {
                            display: inline-block;
                            cursor: pointer;
                            margin-left: 12px;
                            color: #ffffff;
                            .right-icon {
                                font-size: 26px;
                                vertical-align: middle;
                            }
                            .right-img {
                                vertical-align: middle;
                            }
                        }
                    }
                }
            }
        }
    }
    .welbg {
        position: absolute;
        z-index: 1;
        top: 0;
        left: 0;
        right: 0;
        height: 762px;
        overflow: hidden;
        padding-top: 480px;
        margin-top: -480px;
        background: #2d8cf0;
        transform: skewY(-2deg);
        box-shadow: 0 2px 244px 0 rgba(56, 132, 255, 0.4);
        .second {
            position: relative;
            margin-top: 582px;
            height: 220px;
            .bg {
                transform: skewY(-2.5deg);
                display: block;
                position: absolute;
                top: 0;
                left: 0;
                right: 0;
                bottom: 0;
                background: #1F65D6;
            }
        }
    }
    .welcome {
        position: relative;
        z-index: 2;
        height: 700px;
        display: block;
        overflow: hidden;
        color: #FFFFFF;
        padding-top: 480px;
        margin-top: -480px;
        .unslider-arrow {
            display: none;
        }
        .banner {
            padding-top: 60px;
            height: 460px;
            max-width: 1200px;
            margin: 0 auto;
            .banner-carousel {
                max-width: 685px;
                border-radius: 5px;
                overflow: hidden;
            }
            img {
                height: 400px;
                border: 5px solid #fff;
                display: table;
            }
        }
        .z-8 {
            text-align: center;
        }
        h3 {
            color: rgba(255, 255, 255, 0.8);
            font-size: 40px;
            font-weight: normal;
            text-align: center;
            margin: 30px auto;
            width: 380px;
        }
        .start {
            display: inline-block;
            width: 160px;
            height: 50px;
            line-height: 50px;
            text-align: center;
            font-weight: normal;
            cursor: pointer;
            font-size: 20px;
            background: #fff;
            color: #0396f2;
            border-radius: 4px;
            border: 0;

            &:hover, &:focus {
                background: #f6f6f6;
                color: #0396f2;
            }
        }
        .second {
            position: relative;
            height: 220px;
            text-align: center;
            .bg {
                display: block;
                position: absolute;
                top: 0;
                left: 0;
                right: 0;
                bottom: 0;
            }
            .z-row {
                z-index: 2;
                position: relative;
                font-size: 22px;
                max-width: 1400px;
                margin: 0 auto;
                line-height: 220px;
            }
            i {
                color: rgba(255, 255, 255, 0.85);
                margin-right: 6px;
            }
            a {
                color: #fff;
                &:hover, &:visited {
                    color: #fff;
                }
            }

        }
    }

    .block {
        max-width: 1200px;
        margin: 30px auto;
        padding-top: 50px;
        border: 1px solid transparent;

        .wrap-left, .wrap-right {
            line-height: 36px;
            color: #666;
            font-size: 16px;
        }
        .wrap-left {
            margin: 20px 30px 0 0;
        }
        .wrap-right {
            margin: 20px 0 0 30px;
        }
        i {
            color: rgba(248, 14, 21, 0.7);
            margin-right: 6px;
        }
        img {
            border: 5px solid #fff;
            border-radius: 10px;
            width: 100%;
        }
    }

    .p-footer {
        margin: 20px 0;
        text-align: center;
        color: #333;

        a, span {
            color: #333;
            margin-left: 10px;
        }
    }

    @media (max-width: 768px) {
        .header {
            .z-row {
                .header-col-sub {
                    h2 {
                        padding: 12px 0 0 12px;
                        span {
                            display: none;
                        }
                    }
                }
            }
        }
        .welbg {
            display: none;
        }
        .welcome {
            height: auto;
            background: #2d8cf0;
            transform: skewY(-2deg);
            box-shadow: 0 2px 244px 0 rgba(56, 132, 255, 0.4);
            .banner {
                height: auto;
                transform: skewY(2deg);
                .z-row {
                    flex-direction: column;
                    padding-bottom: 52px;
                    > div {
                        width: 90%;
                        margin: 0 auto;
                    }
                }
                h3 {
                    font-size: 24px;
                    width: auto;
                }
                img {
                    max-width: 100%;
                    height: auto;
                }
            }
            .second {
                height: 120px;
                display: flex;
                align-items: center;
                transform: skewY(2deg);
                .bg {
                    transform: skewY(-4.5deg);
                    background: #1F65D6;
                }
                .z-row {
                    height: auto;
                    line-height: 36px;
                    font-size: 16px;
                    display: block;
                    .z-6 {
                        width: 30%;
                        margin: 0 5%;
                        white-space: nowrap;
                    }
                }
            }
        }
        .block {
            flex-direction: column;
            margin: 24px auto;
            padding-top: 24px;
            max-width: 90%;
            border: 0;
            > div {
                width: 96%;
                margin: 0 auto;
            }
            .wrap-left,
            .wrap-right {
                margin: 6px 0;
                line-height: 28px;
            }
        }
    }
}
</style>
<script>
export default {
    data() {
        return {
            loadIng: 0,
            loginShow: false,
            loginType: 'login',

            codeNeed: false,
            codeUrl: $A.apiUrl('users/login/codeimg'),

            formLogin: {
                username: '',
                userpass: '',
                userpass2: '',
                code: '',
            },
            ruleLogin: {},

            systemConfig: $A.jsonParse($A.storage("systemSetting")),

            fromUrl: '',
        }
    },

    mounted() {
        //
    },

    activated() {
        this.getSetting();
        this.fromUrl = decodeURIComponent($A.getObject(this.$route.query, 'from'));
        if (this.fromUrl) {
            this.loginChack();
        }
    },

    deactivated() {
        this.loginShow = false;
    },

    watch: {
        loginShow(val) {
            if (val) {
                this.getSetting();
            } else {
                this.loginType = 'login';
            }
        }
    },

    methods: {
        initLanguage() {
            this.ruleLogin = {
                username: [
                    {required: true, message: this.$L('请填写用户名！'), trigger: 'change'},
                    {type: 'string', min: 2, message: this.$L('用户名长度至少2位！'), trigger: 'change'}
                ],
                userpass: [
                    {required: true, message: this.$L('请填写登录密码！'), trigger: 'change'},
                    {type: 'string', min: 6, message: this.$L('密码错长度至少6位！'), trigger: 'change'}
                ],
                userpass2: [
                    {required: true, message: this.$L('请填写确认密码！'), trigger: 'change'},
                    {type: 'string', min: 6, message: this.$L('确认密码错长度至少6位！'), trigger: 'change'},
                    {
                        validator: (rule, value, callback) => {
                            if (value !== this.formLogin.userpass) {
                                callback(new Error(this.$L('两次密码输入不一致！')));
                            } else {
                                callback();
                            }
                        },
                        required: true,
                        trigger: 'change'
                    },
                ]
            };
        },

        getSetting() {
            $A.apiAjax({
                url: 'system/setting',
                error: () => {
                    $A.storage("systemSetting", {});
                },
                success: (res) => {
                    if (res.ret === 1) {
                        this.systemConfig = res.data;
                        $A.storage("systemSetting", this.systemConfig);
                    } else {
                        $A.storage("systemSetting", {});
                    }
                    if (this.systemConfig.loginWin == 'direct') {
                        this.loginChack();
                    }
                }
            });
        },

        loginChack() {
            if ($A.getToken() !== false) {
                this.goForward({path: '/todo'}, true);
            } else {
                this.loginShow = true;
            }
        },

        refreshCode() {
            this.codeUrl = $A.apiUrl('users/login/codeimg?_=' + Math.random())
        },

        enterpriseOpen() {
            this.goForward({path: '/plans'});
        },

        onBlur() {
            if (this.loginType != 'login') {
                this.codeNeed = false;
                return;
            }
            this.loadIng++;
            $A.ajax({
                url: $A.apiUrl('users/login/needcode'),
                data: {
                    username: this.formLogin.username,
                },
                complete: () => {
                    this.loadIng--;
                },
                success: (res) => {
                    this.codeNeed = res.ret === 1;
                }
            })
        },

        onLogin() {
            this.$refs.login.validate((valid) => {
                if (valid) {
                    this.loadIng++;
                    $A.ajax({
                        url: $A.apiUrl('users/login?type=' + this.loginType),
                        data: this.formLogin,
                        complete: () => {
                            this.loadIng--;
                        },
                        success: (res) => {
                            if (res.ret === 1) {
                                $A.storage("userInfo", res.data);
                                $A.setToken(res.data.token);
                                $A.triggerUserInfoListener(res.data);
                                //
                                this.loadIng--;
                                this.loginShow = false;
                                this.$refs.login.resetFields();
                                this.$Message.success(this.$L('登录成功'));
                                if (this.fromUrl) {
                                    window.location.replace(this.fromUrl);
                                } else {
                                    this.goForward({path: '/todo'}, true);
                                }
                            } else {
                                this.$Modal.error({
                                    title: this.$L("温馨提示"),
                                    content: res.msg
                                });
                                if (res.data.code === 'need') {
                                    this.codeNeed = true;
                                    this.refreshCode();
                                }
                            }
                        }
                    })
                }
            })
        }
    },
}
</script>
