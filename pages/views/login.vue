<template>
	<view>
		<view class="logo-box">
			<image style="width: 200px; height: 200px; background-color: #eeeeee;" :src="src" @error="imageError"></image>
		</view>
		<view class="input-box">
			<text class="text">邮箱：</text>
			<input class="login-input" v-model="loginInfo.mail" focus placeholder="请输入邮箱" />
		</view>
		<view class="input-box">
			<text class="text">密码：</text>
			<input class="login-input" v-model="loginInfo.password" focus placeholder="请输入密码" />
		</view>
		<view class="btn-box">
			<button class="login-btn" type="default" @click="login()">登录</button>
			<button class="login-btn" type="default" @click="register()">注册</button>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				src: 'https://img-cdn-qiniu.dcloud.net.cn/uniapp/images/shuijiao.jpg',
				loginInfo: {
					mail: '',
					password: ''
				}
			}
		},
		onLoad() {

		},
		methods: {
			login() {
				if (this.loginInfo.mail == '' || this.loginInfo.mail == null) {
					uni.showModal({
						content: '请输入邮箱！',
						showCancel:false
					})
				} else if (this.loginInfo.password == '' || this.loginInfo.password == null) {
					uni.showModal({
						content:'请输入密码！',
						showCancel:false
					});
				} else {
					uni.request({
						// url: 'http://localhost:9010/books/allBook/all',
						url: 'http://192.168.0.112:9010/books/allBook/all',
						// data: {
						// 	mian: this.loginInfo.mail,
						// 	password: this.loginInfo.password
						// },
						method: "GET",
						sslVerify:false,
						success: function(res) {
							console.log(res.data);
							uni.switchTab({
							    url: '/pages/views/main'
							});
						},
						fail: function(res) {
							console.log(res.data);
						}
					});
				}
			},
			register(){
				uni.redirectTo({
				    url: 'register'
				});
			}
		}
	}
</script>

<style>
	.login-input {
		border: 1px solid lightgray;
		width: 60%;
		height: 30px;
		border-radius: 5px;
		padding: 5px;
	}

	.text {
		float: left;
		padding: 7px;
	}

	.input-box {
		display: flex;
		justify-content: center;
		height: 50px;
		padding-top: 2%;
	}

	.logo-box {
		display: flex;
		justify-content: center;
		margin: 10%;
	}

	.btn-box {
		margin-top: 20%;
	}

	.login-btn {
		width: 70%;
		border: 1px solid lightgray;
		margin-top: 5%;
	}
</style>
