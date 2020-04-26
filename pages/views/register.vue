<template>
	<view class="register-box">
		<view class="input-box">
			<text class="text">昵称：</text>
			<input class="register-input" v-model="registerInfo.name" focus placeholder="请输入昵称" />
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
			<input class="register-input" v-model="registerInfo.checkPass" focus placeholder="请确认密码" />
		</view>
		<view class="input-box">
			<text class="text">验证码：</text>
			<input class="register-input" v-model="registerInfo.verification" focus placeholder="请输入验证码" />
		</view>
		<view class="btn-box">
			<button class="register-btn" type="default" @click="verify()">获取验证码</button>
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
					password: '',
					checkPass: '',
					verification: ''
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
				if (this.registerInfo.name == '' || this.registerInfo.name == null) {
					uni.showModal({
						content: '请输入昵称！',
						showCancel:false
					})
				} else if (this.registerInfo.mail == '' || this.registerInfo.mail == null) {
					uni.showModal({
						content:'请输入邮箱！',
						showCancel:false
					});
				} else if (this.registerInfo.password == '' || this.registerInfo.password == null) {
					uni.showModal({
						content:'请输入密码！',
						showCancel:false
					});
				} else if (this.registerInfo.checkPass == '' || this.registerInfo.checkPass == null) {
					uni.showModal({
						content:'请确认密码！',
						showCancel:false
					});
				} else if (this.registerInfo.checkPass != this.registerInfo.password) {
					uni.showModal({
						content:'确认密码错误！',
						showCancel:false
					});
				} else if (this.registerInfo.verification == '' || this.registerInfo.verification == null) {
					uni.showModal({
						content:'请输入验证码！',
						showCancel:false
					});
				}
				else{
					console.log(this.registerInfo.password);
					// console.log(md5(this.registerInfo.password));
					uni.request({
						url: 'http://iflag.icube.fun:8080/user/signup/',
						data: {
							email: this.registerInfo.mail,
							code: this.registerInfo.verification,
							password: this.registerInfo.password,
							username:this.registerInfo.name
						},
						method: "POST",			
						header: {
							"Content-Type": "application/x-www-form-urlencoded"
						},
						success: function(res) {
							console.log(res.data);
							if (res.data.StatusCode==0) {
								uni.redirectTo({
									url: '../partner/editPartner'
								});
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
								content: '注册失败，请重试！',
								showCancel:false
							})
						}
					});
				}
			},
			verify(){
				if (this.registerInfo.mail == '' || this.registerInfo.mail == null) {
					uni.showModal({
						content: '请输入邮箱！',
						showCancel:false
					})
				}
				else{
					console.log(this.registerInfo.mail);
					uni.request({
						url: 'http://iflag.icube.fun:8080/verify_code?email='+ this.registerInfo.mail,				
						method: "POST",				
						success: function(res) {
							console.log(res.data);
							if (res.data.StatusCode==0) {
								uni.showModal({
									content: '验证码已发送，5分钟内有效！',
									showCancel:false
								})
							}
							else{
								uni.showModal({
									content: '验证码发送失败，请重试！',
									showCancel:false
								})
							}
						},
						fail: function(res) {
							console.log(res.data);
							uni.showModal({
								content: '验证码发送失败，请重试！',
								showCancel:false
							})
						}
					});
				}
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
