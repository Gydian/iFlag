<template>
	<view>
		<view class="logo-box">
			<image style="width: 200px; height: 200px; background-color: #eeeeee;" :src="src"></image>
		</view>
		<view class="input-box">
			<text class="text">邮箱：</text>
			<input class="login-input" v-model="loginInfo.mail" focus placeholder="请输入邮箱" />
		</view>
		<!-- 		<input class="line" /> -->
		<view class="input-box2">
			<text class="text">密码：</text>
			<input class="login-input" style="outline:none;" v-model="loginInfo.password" placeholder="请输入密码" password="true">
			</input>
			<!--   <view class="iconfont icon-eye"></view> -->
		</view>
		<view class="btn-box">
			<button class="login-btn" type="default" @click="login()">登录</button>
			<button class="login-btn" type="default" @click="register()">注册</button>
		</view>
	</view>
</template>

<script>
	import md5 from 'js-md5';
	export default {
		data() {
			return {
				src: 'https://img-cdn-qiniu.dcloud.net.cn/uniapp/images/shuijiao.jpg',
				loginInfo: {
					mail: '',
					password: ''
				},
				icon: "\ue602"
			}
		},
		onLoad() {},
		methods: {
			login() {
				if (this.loginInfo.mail == '' || this.loginInfo.mail == null) {
					uni.showModal({
						content: '请输入邮箱！',
						showCancel: false
					})
				} else if (this.loginInfo.password == '' || this.loginInfo.password == null) {
					uni.showModal({
						content:'请输入密码！',
						showCancel:false
					});
				} else {
					var that = this;
					uni.request({
						url: 'http://iflag.icube.fun:8080/user/login/',
						data: {
							email: that.loginInfo.mail,
							password: md5(that.loginInfo.password)
						},
						method: "POST",			
						header: {
							"Content-Type": "application/x-www-form-urlencoded"
						},
						success: function(res) {
							console.log(res.data);
							if (res.data.StatusCode==0) {
								uni.setStorage({
									key: 'email',
									data: {mail:that.loginInfo.mail,
										token:res.data.Messenger.token},
									    
									success: function() {
										uni.switchTab({
											url: '/pages/views/main'
										});
									}
								})
							}
							else{
								uni.showModal({
									content: res.data.Messenger,
									showCancel:false
								})
							}
						},
						fail: function(res) {
							console.log(res.data);
							uni.showModal({
								content: '登录失败，请重试！',
								showCancel:false
							})
						}
					});
				} 
			},
			register() {
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
		flex-direction: row;
		justify-content: center;
		height: 50px;
		padding-top: 2%;
		align-items: center;
		/* 		border-bottom: 1px solid lightgray;
		border-top: 1px solid lightgray; */
	}

	.input-box2 {
		display: flex;
		flex-direction: row;
		justify-content: center;
		height: 50px;
		padding-top: 2%;
		align-items: center;
		/* 	border-bottom: 1px solid lightgray; */
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

	.line {
		width: 1;
		height: 1;
		border: 1px solid lightgray;
	}
</style>
