<template>
	<view>
		<tm-fullView bg-color="white" :text="$tm.vx.state().tmVuetify.black">
			<tm-menubars color="white" :shadow="1" title="忘记密码" ></tm-menubars>
			<tm-message ref="msg"></tm-message>
			<view class="pa-50">
				<view :class="[$tm.vx.state().tmVuetify.black ? '' : 'text-black']">
					<view class=" text-size-xl text-weight-b">重置你的密码</view>
				</view>
				<view style="height: 50rpx;"></view>
				<tm-form @submit="sumbit">
					<view>
						<view class="text-size-n">电话号码</view>
						<tm-input :showIndent="$tm.vx.state().tmVuetify.black" name="user" v-model="form.user" required clear :padding="[0, 16]" bg-color="none"
							bgTheme=""  input-type="number" placeholder="请输入电话号码" :border-bottom="!$tm.vx.state().tmVuetify.black">
						</tm-input>
					</view>
					<view class="pt-32">
						<view class="text-size-n">设置登录密码</view>
						<tm-input :showIndent="$tm.vx.state().tmVuetify.black" name="password" :border-bottom="!$tm.vx.state().tmVuetify.black"
							v-model="form.password" required clear :padding="[0, 16]" bg-color="none" bgTheme=""
							inputType="password" placeholder="请输入登录密码"></tm-input>
					</view>
					<view class="pt-32">
						<view class="text-size-n ">验证码</view>
						<tm-input :showIndent="$tm.vx.state().tmVuetify.black" name="code" :border-bottom="!$tm.vx.state().tmVuetify.black"
							v-model="form.code" required clear :padding="[0, 24]" bg-color="none" bgTheme=""
							placeholder="请输入验证码">
							<template v-slot:rightBtn>
								<view>
									<tm-button @click="$refs.daojishi.start()" theme="bg-gradient-primary-lighten-b"
										:round="$tm.vx.state().tmVuetify.black ? 0 : 24" :font-size="16" :height="74"  block :width="190">
										<tm-countdown ref="daojishi" :autoStart="false" :time="60*1000">
											<template v-slot:default="{timeData}">
												<text class="text-size-s ">{{timeData.finish==false&&timeData.data.seconds!=='00'?'剩'+timeData.data.seconds+'秒':'获取验证码'}}</text>
											</template>
										</tm-countdown>
									</tm-button>
								</view>
							</template>
						</tm-input>
					</view>
					<view style="height: 100rpx;"></view>
					<tm-button icon="icon-release" theme="bg-gradient-primary-lighten-b" :iconSize="32" navtieType="form" round="20" fontSize="37" block>修改密码</tm-button>
					
					
				</tm-form>
				<view style="height: 60rpx;"></view>
				
			</view>
		</tm-fullView>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				form: {
					user: '',
					password: '',
					code:''
				}
			};
		},
		methods: {
			//提交表单
			sumbit(e){
				if(!e){
					this.$refs.msg.show({label:'未填写正确',model:'error'})
					return;
				}
				//验证通过，提交表单至后台。
				//请求方法封装在根目录->router->apirouter.js 需要在main.js中引入。
				this.$api.login(this.form.user,this.form.password,this.form.code).then(v=>{
					this.$refs.msg.show({label:'登录成功',model:'success'})
				})
			},
			
		}
	};
</script>

<style></style>
