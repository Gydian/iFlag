<template>
	<view>

		<!-- <image class="avatar" :src="partnerHead"></image> -->
		<uni-fab :content="content" :horizontal="horizontal" :vertical="vertical" :direction="direction" @trigger="trigger"
		 @fabClick="fabClick"></uni-fab>
		<view class="info-1">
			<!-- 布局 -->
			<view style="margin-bottom:70px;">
				<!-- 注释 -->
				<!-- <view class="zs">点击对方头像可进入私聊模式</view> -->
				<view v-for="(item, key) in list" :key="key">
					<view class="left">
						<view>
							<image src="../../static/logo.png" style="width:40px;height:40px;border-radius:50%;border:1px solid #aaa;" />
						</view>
						<view style="margin-left:10px;">
							<view style="font-size:12px;color:#999;">{{ item.ss }}</view>
							<view class="langcon">{{ item.con }}</view>
						</view>
					</view>
				</view>
			</view>
		</view>
	</view>


	</view>
</template>

<script>
	import uniFab from '@/components/uni-fab/uni-fab.vue';
	export default {
		components: {
			uniFab, // 悬浮按钮
		},
		data() {
			return {
				partnerHead: "../../static/logo.png",
				title: 'main',
				horizontal: 'right',
				vertical: 'bottom',
				direction: 'horizontal',
				content: [{
						iconPath: '/static/image/一次性任务.png',
						selectedIconPath: '/static/image/一次性任务.png',
						text: '一次性',
						active: false
					},
					{
						iconPath: '/static/image/周期性任务.png',
						selectedIconPath: '/static/image/周期性任务.png',
						text: '周期性',
						active: false
					},
				],

				list: [
					{
					ss: '对象',
					con: '你今日打卡内容为:'
					}, {
					ss: '对象',
					con: '你今日打卡内容为:'
					}, {
					ss: '对象',
					con: '你今日打卡内容为:'
					}, 
				],
			}
		},


		methods: {
			onNavigationBarButtonTap(e) {
				console.log("success");
				uni.navigateTo({
					url: '../main/calendar'
				});
			},
			fabClick() {
				console.log("success");

			},
			trigger(e) {
				console.log(e)
				// this.content[e.index].active = !e.item.active
				// uni.showModal({
				// 	title: '提示',
				// 	content: `您${this.content[e.index].active ? '选中了' : '取消了'}${e.item.text}`,
				// 	success: function(res) {
				// 		if (res.confirm) {
				// 			console.log('用户点击确定')
				// 		} else if (res.cancel) {
				// 			console.log('用户点击取消')
				// 		}
				// 	}
				// })
				this.content[e.index].active = true
				if (e.item.text == "一次性") {
					console.log("11");
					uni.navigateTo({
						url: '../main/addFlag'
					});
				} else if (e.item.text == "周期性") {
					console.log("22");
					uni.navigateTo({
						url: '../main/addPerFlag'
					});
				}
			},


			Init() {
				var that = this;
				//接口
			},



		},

	}
</script>

<style>
	.avatar {
		height: 50px;
		width: 50px;
		border-radius: 50%;
		-webkit-border-radius: 50%;
		-moz-border-radius: 50%;
		margin-top: 10%;
		margin-left: 5%;
	}

	.bottom {
		position: fixed;
		bottom: 0;
		width: 100%;
		background: #f1f1f1;
		padding-top: 10px;
	}

	.bottom-p {
		height: 37px;
		display: flex;
		justify-content: space-between;
		flex-wrap: nowrap;
	}

	.bottom-p view:nth-child(1) {
		width: 77%;
		margin-left: 10px;
		background-color: #ffffff;
		border-radius: 40px;
		padding: 7px 0 0 10px;
	}

	.bottom-p view:nth-child(2) {
		width: 20%;
		margin-left: 5px;
		font-size: 14px;
		text-align: center;
		height: 37px;
		margin-right: 10px;
		border-radius: 40px;
		color: #ffffff;
		background: #25b0f3;
		line-height: 37px;
	}

	.center {
		text-align: center;
		font-size: 12px;
		color: #aaa;
		margin-top: 10px;
		letter-spacing: 1px;
	}

	.left {
		display: flex;
		flex-wrap: nowrap;
		justify-content: flex-start;
		margin-left: 10px;
		margin-top: 10px;
	}

	.right {
		display: flex;
		flex-wrap: nowrap;
		justify-content: flex-end;
		margin-right: 10px;
		margin-top: 10px;
	}

	.langcon {
		border: 1px solid #333333;
		font-size: 14px;
		color: #414141;
		height: 30px;
		line-height: 30px;
		border-radius: 7px;
		margin-top: 5px;
		text-align: right;
		padding: 3px 10px 3px 10px;
		background: #f8f8f8;
	}

	/* 	.zs {
			font-size: 12px;
			letter-spacing: 1px;
			color: #AAAAAA;
			margin-top: 5px;
			text-align: center;
		} */

	/* 分类 */
	.classification {
		display: flex;
		flex-wrap: nowrap;
		justify-content: space-between;
		width: 90%;
		margin: 0 auto;
		padding-top: 10px;
	}

	.classification image {
		width: 21px;
		height: 21px;
	}
</style>
