<template>
	<view>
		<view>
			<view class="avatar-view">
				<view @click="changeAvatar">
					<image class="avatar" src="../../static/logo.png"></image>
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
			<navigator url="redirect/redirect?title=redirect" open-type="redirect">
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
			<navigator url="redirect/redirect?title=redirect" open-type="redirect">
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
			<button class="logout-btn">退出登录</button>
		</view>
	</view>
</template>

<script>
	import uniPopup from "@/components/uni-popup/uni-popup.vue"

	export default {
		data() {
			return {
				name: '昵称'
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
			changeAvatar() {}
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
