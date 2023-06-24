<template>
	<view class="page">
		<view class="top-background-raduis"> </view>
		<view class="login-box">
			<view class="login-card-title">
				<view @click="isTarget = true" :class="['login-card-title-content', isTarget ? 'title-target' : '']">
					<view :class="['before-view', isTarget ? 'before-left' : '']"></view>
					账号密码登录
				</view>
				<view @click="isTarget = false" :class="['login-card-title-content', isTarget == false ? 'title-target' : '']">
					<view :class="['before-view', isTarget == false ? 'before-right' : '']"></view>
					账号注册
				</view>
			</view>
			<!-- 需要安装 uni-easyinput 组件 -->
			<view class="login-card-input">
				<uni-easyinput
					class="hj-username"
					v-model="usernameValue"
					focus
					maxlength="30"
					:trim="true"
					@focus="UserNamePassWrodFocusBorderStyle"
					:styles="inputColorConfigToUserName"
					@blur="UserNamePassWrodBlurBorderStyle"
					placeholder="请输入账号"></uni-easyinput>
				<uni-easyinput
					class="hj-password"
					v-model="passwordValue"
					type="password"
					@focus="PassWordFocusBorderStyle"
					@blur="PassWordBlurBorderStyle"
					:styles="inputColorConfigToPassWord"
					placeholder="请输入密码"></uni-easyinput>
			</view>
			<view class="login-card-loginIn">
				<view class="login-card-loginIn-btn"> {{ isTarget ? "登录" : "注册" }} </view>
			</view>
			<view class="login-bottom-info"> 忘记密码 </view>
		</view>
		<view class="bottom-authorize">
			<image src="@/static/weixin.png"></image>
			<image src="@/static/qq.png"></image>
			<image src="@/static/github.png"></image>
			<image src="@/static/weibo.png"></image>
		</view>
		<label class="bottom-message">
			<checkbox value="cb" checked="true" color="#3cb79a" style="transform: scale(0.7)" />登录/注册即代表您已同意《<text class="textDecoration">麻丝使用协议</text>》</label
		>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				// 输入框边框颜色默认值
				colorDefault: "#b4b4b4",
				// 输入框边框聚焦值
				colorFocusColor: "#3cb79a",
				// 输入框颜色配置
				inputColorConfigToUserName: {
					color: "#000000",
					disableColor: "#eee",
					borderColor: "#b4b4b4"
				},
				// 输入框颜色配置
				inputColorConfigToPassWord: {
					color: "#000000",
					disableColor: "#eee",
					borderColor: "#b4b4b4"
				},
				// 是否被点击切换登录和注册
				isTarget: true,
				usernameValue: "",
				passwordValue: ""
			}
		},
		//监听页面初始化，其参数同 onLoad 参数，为上个页面传递的数据，参数类型为 Object（用于页面传参），触发时机早于 onLoad
		onInit() {},
		//监听页面加载，其参数为上个页面传递的数据，参数类型为 Object（用于页面传参）
		onLoad() {},
		//监听页面初次渲染完成。注意如果渲染速度快，会在页面进入动画完成前触发
		onReady() {},
		//监听页面显示。页面每次出现在屏幕上都触发，包括从下级页面点返回露出当前页面
		beforeDestroy() {},
		//页面滚动到底部的事件（不是scroll-view滚到底），常用于下拉下一页数据。
		onReachBottom() {},
		onShareAppMessage() {},
		created() {},
		methods: {
			// 输入框聚焦变换边框样式
			UserNamePassWrodFocusBorderStyle() {
				this.inputColorConfigToUserName.borderColor = this.colorFocusColor
			},
			PassWordFocusBorderStyle() {
				this.inputColorConfigToPassWord.borderColor = this.colorFocusColor
			},
			// 输入框失去焦点变换边框样式
			UserNamePassWrodBlurBorderStyle() {
				this.inputColorConfigToUserName.borderColor = this.colorDefault
			},
			PassWordBlurBorderStyle() {
				this.inputColorConfigToPassWord.borderColor = this.colorDefault
			}
		}
	}
</script>
<style lang="scss" scoped>
	.page {
		position: relative;
		height: 100vh;

		.top-background-raduis {
			height: 30%;
			background-color: #d8d8d8;
			border-bottom-left-radius: 50%;
			border-bottom-right-radius: 50%;
		}
		.login-box {
			position: absolute;
			top: 20%;
			left: 10%;
			right: 10%;
			width: 80%;
			height: 550rpx;
			padding: 30rpx;
			background-color: white;
			box-shadow: 9px 9px 6px #e3e3e3;
			border-radius: 40rpx;
			box-sizing: border-box;

			.login-card-title {
				height: 20%;
				padding: 10rpx;
				display: flex;
				justify-content: space-around;
				box-sizing: border-box;

				.login-card-title-content {
					position: relative;
					font-size: 1.2em;
					color: #3cb79a;
				}
				.title-target {
					color: #3cb79a;
				}
				.before-view {
					position: absolute;
					left: 50%;
					height: 3px;
					width: 0px;
					bottom: 0;
					border-bottom: 3px solid #3cb79a;
					transition: 0.7s;
					transform: translate(-50%, 0);
				}
				.before-left {
					width: 60px;
				}
				.before-right {
					width: 60px;
				}
			}

			.login-card-input {
				display: flex;
				flex-direction: column;
				justify-content: center;
				height: 40%;
				margin-top: 30rpx;
				padding: 5rpx;
			}

			.login-card-loginIn {
				height: 25%;
				display: flex;
				justify-content: center;
				padding: 10rpx;
				align-items: center;
				box-sizing: border-box;

				.login-card-loginIn-btn {
					display: flex;
					align-items: center;
					justify-content: center;
					background-color: #3cb79a;
					width: 100%;
					font-size: 1.2em;
					height: 80%;
					border-radius: 40rpx;
					color: white;
				}
			}
			.login-bottom-info {
				display: flex;
				justify-content: flex-end;
				height: 5%;
				font-size: 0.7em;
				color: #3cb79a;
			}
		}
		.bottom-authorize {
			position: absolute;
			bottom: 140rpx;
			left: 0;
			right: 0;
			margin: 0 auto;
			display: flex;
			justify-content: space-around;
			width: 80%;
			height: 100rpx;
			overflow: hidden;
			align-items: center;
			image {
				width: 100rpx;
				height: 100rpx;
			}
		}
		.bottom-message {
			position: absolute;
			height: 70rpx;
			display: flex;
			width: 100%;
			justify-content: center;
			align-items: center;
			font-size: 0.9em;
			overflow: hidden;
			color: #969898;
			bottom: 40rpx;
			.textDecoration {
				color: #3cb79a;
				text-decoration: underline #3cb79a;
			}
		}
	}
</style>
