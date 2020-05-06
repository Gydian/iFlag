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
		onShow: function(){
			var that = this;
			//接口
			uni.getStorage({
				key: 'email',
				success: function(res) {
					console.log('这是key中的内容：' + res.data)
					uni.request({
						url: 'http://iflag.icube.fun:8080/onetime/findById/' + res.data,
						method: "GET",
						sslVerify: false,
						success: function(response) {
							console.log(response)
							console.log("试一试")
							//that.list.add(response.data.content)
							// that.list.ss = '对象'
							// that.list.con = response.data.content
							// console.log("试完了")
						},
						fail: function(response) {
							console.log(response.data);
						}
					});
				}
			})
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
				uni.getStorage({
					key: 'email',
					success: function(res) {
						console.log('这是key中的内容：' + res.data)
						uni.request({
							url: 'http://iflag.icube.fun:8080/onetime/findById/' + res.data,
							method: "GET",
							sslVerify: false,
							success: function(response) {
								console.log(response)
								console.log("试一试")
								that.list.add(response.data.content)
								// that.time1 = response.data.setRemindTime
								// that.checkedVal1 = response.data.isSetFlag
								// that.cycleVal1 = response.data.setRepeatPeriod
								// that.time2 = response.data.delRemindTime
								// that.checkedVal2 = response.data.isDelF
								// that.cycleVal2 = response.data.delRepeatPeriod
							},
							fail: function(response) {
								console.log(response.data);
							}
						});
					}
				})
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


</style>
