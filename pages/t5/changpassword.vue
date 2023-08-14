<template>
	<view>
		<tm-fullView bg-color="white" :text="$tm.vx.state().tmVuetify.black">
			<tm-menubars color="white" :shadow="1" title="修改密码" ></tm-menubars>
			<tm-message ref="msg"></tm-message>
			<view class="pa-50">
				<view :class="[$tm.vx.state().tmVuetify.black ? '' : 'text-black']">
					<view class=" text-size-xl text-weight-b">修改登录密码</view>
				</view>
				<view style="height: 50rpx;"></view>
				<tm-form @submit="sumbit">
					
					<view class="pt-32">
						<view class="text-size-n">登录密码</view>
						<tm-input :showIndent="$tm.vx.state().tmVuetify.black" name="oldpassword" :border-bottom="!$tm.vx.state().tmVuetify.black"
							v-model="form.oldpassword" required clear :padding="[0, 16]" bg-color="none" bgTheme=""
							inputType="password" placeholder="请输入登录密码"></tm-input>
					</view>
					<view class="pt-32">
						<view class="text-size-n">新的密码</view>
						<tm-input :showIndent="$tm.vx.state().tmVuetify.black" name="password" :border-bottom="!$tm.vx.state().tmVuetify.black"
							v-model="form.password" required clear :padding="[0, 16]" bg-color="none" bgTheme=""
							inputType="password" placeholder="请输入登录密码"></tm-input>
					</view>
					<view style="height: 100rpx;"></view>
					<tm-button theme="bg-gradient-primary-lighten-b" icon="icon-release" :iconSize="32" navtieType="form" round="20" fontSize="37" block>确认修改</tm-button>
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
					oldpassword: '',
					password: ''
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
			},
			
		}
	};
</script>

<style></style>
