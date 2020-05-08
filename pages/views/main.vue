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
					<view v-if="item.finish == false">
						<view class="left">
							<view>
								<!-- <image src="../../static/logo.png" style="width:40px;height:40px;border-radius:50%;border:1px solid #aaa;" /> -->
								<!-- <image src="this.partnerHead" style="width:40px;height:40px;border-radius:50%;border:1px solid #aaa;" /> -->
								<image class="avatar" :src="partnerHead"></image>
							</view>
							<view style="margin-left:10px;">
								<view style="font-size:12px;color:#999;">{{ header }}</view>
								<!-- <view style="font-size:12px;color:#999;">{{ item.finish }}</view> -->
								<view class="langcon">{{ item.content }}</view>
							</view>
						</view>
					</view>
				</view>
				<!-- 注释 -->
				<!-- <view class="zs">点击对方头像可进入私聊模式</view> -->
				<view v-for="(item, key) in list2" :key="key">
					<view class="left">
						<view>
							<!-- <image src="../../static/logo.png" style="width:40px;height:40px;border-radius:50%;border:1px solid #aaa;" /> -->
							<image class="avatar" :src="partnerHead"></image>
						</view>
						<view style="margin-left:10px;">
							<view style="font-size:12px;color:#999;">{{ header }}</view>
							<view class="langcon">{{ item.content }}</view>
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
						iconPath: '/static/image/once.png',
						selectedIconPath: '/static/image/once.png',
						text: '一次性',
						active: false
					},
					{
						iconPath: '/static/image/periodic.png',
						selectedIconPath: '/static/image/periodic.png',
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
				list2: [
					{
					ss: '对象',
					con: '你今日打卡内容为:'
					},
				],
				header: '对象',
			}
		},
		onShow: function(){
			var that = this;
			//接口
			uni.getStorage({
				key: 'email',
				success: function(res) {
					that.token = res.data.token;
					console.log('这是key中的内容：' + res.data.userid)
					uni.request({
						url: 'http://iflag.icube.fun:8080/onetime/findByUserid/' + res.data.userid,
						method: "GET",
						sslVerify: false,
						success: function(response) {
							console.log(response)
							console.log("试一试")
							that.list = response.data
						},
						fail: function(response) {
							console.log(response.data);
						}
					});
					
					uni.request({
						url: 'http://iflag.icube.fun:8080/periodic/findByUserid/' + res.data.userid,
						method: "GET",
						sslVerify: false,
						success: function(response) {
							console.log(response)
							console.log("试一试2")
							that.list2 = response.data
						},
						fail: function(response) {
							console.log(response.data);
						}
					});
					
					/// 头像
					uni.request({
						url: 'http://iflag.icube.fun:8080/user/object/'+res.data.token,
						method: "GET",
						sslVerify:false,
						success: function(response) {
							console.log(response.data);
							that.partnerHead = "http://59.110.64.233:8080/user/object/"+res.data.token+'?pwd='+getRandom(0, 100);
							console.log(that.partnerHead);
						},
						fail: function(response) {
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
		},
	}
	function getRandom(start, end, fixed=0) {
	            let differ = end - start
	            let random = Math.random()
	            return (start + differ * random).toFixed(fixed)
	}
</script>

<style>
	.avatar {
		height: 40px;
		width: 40px;
		border-radius: 50%;
		border: 1px solid #aaa;
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