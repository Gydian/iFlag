<template>
	<view class="register-box">
		<view class="input-box">
			<text class="text">昵称：</text>
			<input class="register-input" v-model="registerInfo.name" focus placeholder="请输入邮箱" />
		</view>
		<view class="input-box">
			<text class="text">邮箱：</text>
			<input class="register-input" v-model="registerInfo.mail" focus placeholder="请输入邮箱" />
		</view>
		<view class="input-box">
			<text class="text">密码：</text>
			<input class="register-input" v-model="registerInfo.password" focus placeholder="请输入密码" />
		</view>
		<view class="input-box">
			<text class="text">确认密码：</text>
			<input class="register-input" v-model="registerInfo.checkPass" focus placeholder="请输入密码" />
		</view>
		<view class="input-box">
			<text class="text">验证码：</text>
			<input class="register-input" v-model="registerInfo.password" focus placeholder="请输入密码" />
		</view>
		<view class="btn-box">
			<button class="register-btn" type="default" @click="l">获取验证码</button>
			<button class="register-btn" type="default" @click="register()">注册</button>
			<button class="register-btn" type="default" @click="login()">返回登录</button>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				registerInfo: {
					name: '',
					mail: '',
					password: ''
				}
			}
		},
		methods: {
			login(){
				uni.redirectTo({
				    url: 'login'
				});
			},
			register(){
			}
		}
	}
</script>

<style>
	.register-input {
		border: 1px solid lightgray;
		width: 60%;
		height: 30px;
		border-radius: 5px;
		padding: 5px;
	}

	.text {
		float: left;
		padding: 10px;
		width: 100px;
		font-size: 18px;
	}

	.input-box {
		display: flex;
		justify-content: center;
		height: 50px;
		padding-top: 2%;
	}

	.register-box {
		margin-top: 15%;
	}
	
	.btn-box {
		margin-top: 20%;
	}
	
	.register-btn {
		width: 70%;
		border: 1px solid lightgray;
		margin-top: 5%;
	}
</style>
