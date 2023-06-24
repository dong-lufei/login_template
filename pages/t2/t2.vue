<template>
	<!-- 自定义导航栏 -->
	<!-- #ifndef H5 -->
	<u-navbar :is-back="false" :title-bold="true" title="登录" :background="{ background: '#fff' }"> </u-navbar>
	<!-- #endif -->

	<view class="login-wrap">
		<!-- 登录上方菜单栏 -->
		<view class="loginTop">
			<view class="loginTopCode" v-if="loginMode === 1" @click="loginCode">切换验证码登录</view>
			<view class="loginTopCode" v-else-if="loginMode === 2" @click="loginUsername">切换账号密码登录</view>
		</view>
		<!-- 登录框 -->
		<view class="loginBox">
			<view class="logoImgBox">
				<image style="width: 100%; height: 100%" :src="loginImage"></image>
			</view>
			<view class="loginInput">
				<input type="text" v-if="loginMode === 1" class="username inp" placeholder="请输入用户名/手机号" maxlength="11" @input="phoneInput" />
				<input type="number" v-else-if="loginMode === 2" class="username inp" placeholder="请输入手机号" maxlength="11" @input="phoneInput" />
			</view>
			<view class="loginInput" v-if="loginMode === 1">
				<input type="password" class="password inp" placeholder="请输入密码" maxlength="16" @input="passwordInput" />
			</view>
			<view class="loginInput loginInputCode" v-else-if="loginMode === 2">
				<input type="number" class="code inp" placeholder="请输入验证码" maxlength="4" @input="codeInput" />
				<view class="getPhoneCode" @click="getCode">{{ codeButton }}</view>
			</view>
			<view class="loginInput login" @click="login"> 登录 </view>
		</view>
		<view class="other">
			<!-- 其他登录方式 -->
			<view class="otherLoginTitle">-------------- 其他一键登录方式 --------------</view>
			<view class="otherLogin">
				<view class="loginProvider weiixnLogin" @click="weixinlogin">
					<image style="width: 100%; height: 100%" src="@/static/weixin.png"></image>
				</view>
				<view class="loginProvider qqLogin" @click="qqlogin">
					<image style="width: 100%; height: 100%" src="@/static/qq2.png"></image>
				</view>
				<view class="loginProvider weiboLogin" @click="weibologin">
					<image style="width: 100%; height: 100%" src="@/static/github.png"></image>
				</view>
				<view class="loginProvider weiboLogin" @click="weibologin">
					<image style="width: 100%; height: 100%" src="@/static/weibo.png"></image>
				</view>
			</view>
		</view>

		<!-- 忘记密码/新用户注册 -->
		<view class="bottomBox">
			<view class="forgetPWD">忘记密码</view>
			<view class="register">用户注册</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				loginMode: 1, // 1代表账号密码模式，2代表手机号验证码模式
				loginImage: "../../static/logo.png", // 换成自己项目的 logo
				codeClick: true, // 是否点击了获取手机短信验证码中
				phone: "", // 用户名或手机号
				password: "", // 密码
				code: "", // 验证码
				codeButton: "获取验证码" // 验证码按钮的状态内容
			}
		},
		methods: {
			loginCode() {
				this.loginMode = 2
			},
			loginUsername() {
				this.loginMode = 1
			},
			// 手机号输入
			phoneInput(e) {
				this.phone = e.detail.value
			},
			// 密码输入
			passwordInput(e) {
				this.password = e.detail.value
			},
			// 验证码输入
			codeInput(e) {
				this.code = e.detail.value
			},
			// 点击登录
			login() {
				var _that = this
				if (_that.phone === "") {
					uni.showToast({
						title: "请输入用户名",
						icon: "none",
						duration: 1000
					})
				} else if (_that.password === "" && _that.loginMode === 1) {
					uni.showToast({
						title: "请输入密码",
						icon: "none",
						duration: 1000
					})
				} else if (_that.code === "" && _that.loginMode === 2) {
					uni.showToast({
						title: "请输入验证码",
						icon: "none",
						duration: 1000
					})
				} else {
					//这里请求接口
					console.log(_that.phone, _that.password, _that.code)
				}
			},
			// 获取验证码
			getCode() {
				var _that = this
				console.log("getCode", _that.codeClick)
				// const mobsms = uni.requireNativePlugin("WB-MobSms")
				// 倒计时
				if (_that.codeClick) {
					if (_that.phone[0] != 1 || _that.phone.length != 11) {
						uni.showToast({
							title: "请输入正确的手机号",
							icon: "none",
							duration: 800
						})
					} else {
						_that.codeClick = false
						// // 发送短信
						// mobsms.sendSms({ zone: "86", phone_number: _that.phone }, (result) => {
						// 	if (result.status === "success") {
						// 		uni.showToast({
						// 			title: "验证码获取中",
						// 			icon: "loading",
						// 			duration: 800
						// 		})
						// 	}
						// })
						// 倒计时开始
						var time = 60
						var timeId = setInterval(function () {
							_that.codeButton = "填入验证码 " + time
							time--
							if (time <= 0) {
								_that.codeButton = "重新发送"
								_that.codeClick = true
								clearInterval(timeId)
							}
						}, 1000)
					}
				}
			},
			// 微信登录
			weixinlogin() {
				uni.login({
					provider: "weixin",
					success: function (e) {
						console.log("【获取 code 的 e】：", e)

						// 获取用户信息
						uni.request({
							url: `http://localhost:3000/getToken?code=${e.code}`,
							success: function (res) {
								console.log("【后端响应数据】:", res)
							}
						})
					}
				})
			},
			// qq登录
			qqlogin() {
				// var _that = this
				uni.login({
					provider: "qq",
					success: function () {
						uni.getUserInfo({
							provider: "qq",
							success: function (infoRes) {
								console.log(infoRes)
								// _that.loginImage = infoRes.userInfo.figureurl_qq_2
							}
						})
					}
				})
			},
			// 微博登录
			weibologin() {
				uni.login({
					provider: "sinaweibo",
					success: function (loginRes) {
						console.log(loginRes)
					}
				})
			}
		}
	}
</script>

<style>
	page {
		background-color: #f1f1f1;
		font-size: 28rpx;
		color: #333;
		font-family: Helvetica Neue, Helvetica, sans-serif;
	}

	.loginTop {
		display: flex;
		justify-content: flex-end;
		margin: 10rpx 20rpx 0 0;
	}

	.loginTopCode {
		color: #999;
	}

	.loginBox {
		width: 650rpx;
		margin: 60rpx auto;
	}

	.logoImgBox {
		width: 200rpx;
		height: 200rpx;
		margin: 0 auto 50rpx;
	}

	.loginInput {
		width: 500rpx;
		height: 80rpx;
		line-height: 80rpx;
		margin: 20rpx auto 0;
		border-radius: 10rpx;
		background: #fff;
	}

	.inp {
		height: 100%;
		font-size: 30rpx;
		text-indent: 0.5em;
	}

	.username {
		background: url("@/static/shouji.png") no-repeat;
		background-position: 10rpx;
		padding-left: 65rpx;
	}

	.password {
		background: url("@/static/mima.png") no-repeat;
		background-position: -2rpx;
		background-size: 74rpx;
		padding-left: 66rpx;
	}

	.loginInputCode {
		display: flex;
	}

	.code {
		width: 300rpx;
		padding-left: 65rpx;
		background: url("@/static/code.png") no-repeat;
		background-position: 9rpx;
		background-size: 54rpx;
	}

	.getPhoneCode {
		width: 200rpx;
		border-radius: 10rpx;
		line-height: 85rpx;
		text-align: center;
		color: #fff;
		background: #333;
	}

	.login {
		display: flex;
		justify-content: center;
		height: 80%;
		margin-top: 20rpx;
		border-radius: 40rpx;
		color: white;
		background-color: #3cb79a;
		font-size: 1.2em;
	}

	.other {
		position: fixed;
		bottom: 200rpx;
		left: 0;
		right: 0;
	}

	.otherLoginTitle {
		color: #999;
		margin-bottom: 20rpx;
		text-align: center;
		font-size: 28rpx;
	}

	.otherLogin {
		width: 500rpx;
		height: 95rpx;
		margin: 0 auto;
		display: flex;
		justify-content: space-between;
	}

	.loginProvider {
		width: 95rpx;
		height: 95rpx;
		background: #fff;
		border-radius: 95rpx;
	}

	.bottomBox {
		display: flex;
		justify-content: center;
		position: fixed;
		bottom: 60rpx;
		left: 0;
		right: 0;
		height: 30rpx;
		line-height: 30rpx;
		font-size: 32rpx;
	}

	.forgetPWD {
		color: #999;
		border-right: 1rpx solid #bbb;
		padding-right: 15rpx;
	}

	.register {
		color: #999;
		padding-left: 15rpx;
	}
</style>
