<template>
	<view>
		<view>
			<view class="avatar-view">
				<view @click="changeDoc">
					<image class="avatar" :src="photo"></image>
				</view>
				<view class="name" @click="changeDoc">{{name}}</view>
				<view @click="changeDoc">
					<image class="enter-icon" src="../../static/image/向右.png"></image>
				</view>
			</view>
			<button class="navigate-btn" @click="checkUpdate()">
				<view>
					<image class="icon" src="../../static/image/更新.png"></image>
				</view>
				<view class="text">检查更新</view>
				<view>
					<image class="enter-icon" src="../../static/image/向右.png"></image>
				</view>
			</button>
			<uni-popup ref="popup" type="bottom">
				<view>
					<button class="popup-btn">
						<view class="popup-text">已是最新版本哦！</view>
					</button>
				</view>
			</uni-popup>
			<navigator url="../personal/help" open-type="navigate">
				<button class="navigate-btn">
					<view>
						<image class="icon" src="../../static/image/帮助.png"></image>
					</view>
					<view class="text">帮助与反馈</view>
					<view>
						<image class="enter-icon" src="../../static/image/向右.png"></image>
					</view>
				</button>
			</navigator>
			<navigator url="../personal/remind" open-type="navigate">
				<button class="navigate-btn">
					<view>
						<image class="icon" src="../../static/image/闹钟.png"></image>
					</view>
					<view class="text">打卡提醒</view>
					<view>
						<image class="enter-icon" src="../../static/image/向右.png"></image>
					</view>
				</button>
			</navigator>
			<navigator url="../personal/about" open-type="navigate">
				<button class="navigate-btn">
					<view>
						<image class="icon" src="../../static/image/关于.png"></image>
					</view>
					<view class="text">关于我们</view>
					<view>
						<image class="enter-icon" src="../../static/image/向右.png"></image>
					</view>
				</button>
			</navigator>
			<navigator url="../personal/achievement" open-type="navigate">
				<button class="navigate-btn">
					<view>
						<image class="icon" src="../../static/image/分享－理财成就.png"></image>
					</view>
					<view class="text">我的成就</view>
					<view>
						<image class="enter-icon" src="../../static/image/向右.png"></image>
					</view>
				</button>
			</navigator>
		</view>
		<view>
			<button class="logout-btn" @click="logout()">退出登录</button>
		</view>
	</view>
</template>

<script>
	import uniPopup from "@/components/uni-popup/uni-popup.vue"

	export default {
		data() {
			return {
				name: '昵称',
				photo:'../../static/logo.png'
			}
		},
		components: {
			uniPopup,
		},
		methods: {
			checkUpdate() {
				this.$refs.popup.open()
			},
			changeDoc() {
				uni.navigateTo({
					url: '../personal/perDoc'
				});
			},
			logout() {
				uni.showModal({
					content: '确定要退出登录吗？',
					success: function(res) {
						if (res.confirm) {
							uni.redirectTo({
								url: './login'
							})
						} else if (res.cancel) {
							
						}
					}
				})
			},
			getInfo(){
				var that = this
				uni.getStorage({
					key:'email',
					success:function(res){
						console.log('这是key中的内容：'+res.data)
						uni.request({
							url: 'http://59.110.64.233:8080/user/findByEmail/'+res.data,
							method: "GET",
							sslVerify:false,
							success: function(response) {
								console.log(response)
								that.name=response.data.username
								that.photo=response.data.photo
							},
							fail: function(response) {
								console.log(response.data);
							}
						});
					}
				})
			}
		},
		onLoad : function(){
			this.getInfo()
		},
		onShow:function(){
			this.getInfo()
		}
	}
</script>

<style>
	.avatar {
		height: 60px;
		width: 60px;
		border-radius: 50%;
		-webkit-border-radius: 50%;
		-moz-border-radius: 50%;
		float: left;
		margin: 1%;
	}

	.name {
		float: left;
		margin: 6%;
	}

	.enter-icon {
		height: 25px;
		width: 25px;
		float: right;
		margin: 5%;
	}

	.navigate-btn {
		border: 0px;
		border-radius: 0;
		height: 60px;
		background-color: #fff;
	}

	button::after {
		border: none;
	}

	.avatar-view {
		height: 70px;
		padding-left: 14px;
		padding-right: 14px;
		border-bottom: 1px solid lightgray;
		border-radius: 0;
		background-color: #fff;
	}

	.icon {
		height: 30px;
		width: 30px;
		float: left;
		margin: 4%;
	}

	.text {
		float: left;
		margin: 2%;
	}

	.popup-btn {
		height: 100px;
	}

	.popup-text {
		margin-top: 25px;
	}

	.logout-btn {
		border: 1px solid lightgray;
		margin-top: 40%;
	}
</style>
