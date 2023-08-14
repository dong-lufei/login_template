<template>
	<view>
		<!-- 依赖 tmui 组件 -->
		<tm-fullView bg-color="white" :text="$tm.vx.state().tmVuetify.black">
			<tm-menubars color="white" :shadow="1" title="登录应用" :showback="false"></tm-menubars>
			<tm-message ref="msg"></tm-message>
			<view class="pa-50">
				<view :class="[$tm.vx.state().tmVuetify.black ? '' : 'text-black']">
					<view class="text-size-xl text-weight-b">您好，</view>
					<view class="text-size-xl text-weight-b">欢迎来到XXX公司</view>
				</view>
				<view style="height: 50rpx"></view>
				<tm-form @submit="sumbit">
					<view>
						<view class="text-size-n">电话号码</view>
						<tm-input
							:showIndent="$tm.vx.state().tmVuetify.black"
							name="user"
							v-model="form.user"
							required
							clear
							:padding="[0, 16]"
							bg-color="none"
							bgTheme=""
							input-type="number"
							placeholder="请输入电话号码"
							:border-bottom="!$tm.vx.state().tmVuetify.black">
						</tm-input>
					</view>
					<view class="pt-32">
						<view class="text-size-n">登录密码</view>
						<tm-input
							:showIndent="$tm.vx.state().tmVuetify.black"
							name="password"
							:border-bottom="!$tm.vx.state().tmVuetify.black"
							v-model="form.password"
							required
							clear
							:padding="[0, 16]"
							bg-color="none"
							bgTheme=""
							inputType="password"
							placeholder="请输入登录密码"></tm-input>
					</view>
					<view class="pt-32">
						<view class="text-size-n">验证码</view>
						<tm-input
							:showIndent="$tm.vx.state().tmVuetify.black"
							name="code"
							:border-bottom="!$tm.vx.state().tmVuetify.black"
							v-model="form.code"
							required
							clear
							:padding="[0, 24]"
							bg-color="none"
							bgTheme=""
							placeholder="请输入验证码">
							<template v-slot:rightBtn>
								<view>
									<tm-button
										theme="bg-gradient-primary-lighten-b"
										@click="$refs.daojishi.start()"
										:round="$tm.vx.state().tmVuetify.black ? 0 : 24"
										:font-size="16"
										:height="74"
										block
										:width="190">
										<tm-countdown ref="daojishi" :autoStart="false" :time="60 * 1000">
											<template v-slot:default="{ timeData }">
												<text class="text-size-s">{{
													timeData.finish == false && timeData.data.seconds !== "00" ? "剩" + timeData.data.seconds + "秒" : "获取验证码"
												}}</text>
											</template>
										</tm-countdown>
									</tm-button>
								</view>
							</template>
						</tm-input>
					</view>
					<view style="height: 100rpx"></view>
					<tm-button theme="bg-gradient-primary-lighten-b" icon="icon-lock" :iconSize="32" navtieType="form" round="20" fontSize="37" block>登录</tm-button>
					<view class="flex-between py-32 text-size-n fulled">
						<navigator url="changpassword"><text>修改密码</text></navigator>
						<navigator url="forgetpassword"><text>忘记密码</text></navigator>
						<view>
							<tm-helpTips label="如果对帐户有疑问,请寻求客服帮助.">
								<text>帮助?</text>
							</tm-helpTips>
						</view>
						<view @click="$tm.theme.setBlack()">切换暗黑</view>
					</view>
				</tm-form>
				<view style="height: 60rpx"></view>
				<tm-divider :color="$tm.vx.state().tmVuetify.black ? 'grey-darken-4' : 'grey-lighten-1'" text="其它社交帐号登录"></tm-divider>
				<view class="flex-center pt-50">
					<tm-button size="s" theme="green" iconSize="36" icon="icon-weixin" fab round="rouned"></tm-button>
					<tm-button size="s" theme="blue" iconSize="36" icon="icon-QQ" fab round="rouned"></tm-button>
					<tm-button size="s" theme="red" iconSize="36" icon="icon-weibo" fab round="rouned"></tm-button>
					<tm-button size="s" theme="black" iconSize="36" icon="icon-ios" fab round="rouned"></tm-button>
				</view>
			</view>
		</tm-fullView>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				form: {
					user: "",
					password: "",
					code: "",
				},
			}
		},
		methods: {
			//提交表单
			sumbit(e) {
				if (!e) {
					this.$refs.msg.show({ label: "未填写正确", model: "error" })
					return
				}
				//验证通过，提交表单至后台。
				//请求方法封装在根目录->router->apirouter.js 需要在main.js中引入。
				this.$api.login(this.form.user, this.form.password, this.form.code).then((v) => {
					this.$refs.msg.show({ label: "登录成功", model: "success" })
				})
			},
		},
	}
</script>

<style></style>
