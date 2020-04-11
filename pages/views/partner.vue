<template>
	<view style="text-align: center;">
		<image class="avatar" :src="partnerHead"></image>
		<view class="name">昵称：{{name}}</view>
		<view class="qinmidu">
			<text class="iconfont icon-bangzhu2" @click="detail"></text>
			亲密度
			<view style="width: 220px; margin-left: 5%;">
				<cmd-progress :percent="percent" :stroke-width="24" stroke-shape="square"></cmd-progress>
			</view>
			
		</view>
		<view class="tips">
			<uni-card title="Tips">
			    {{tips}}
			</uni-card>
		</view>
		
	</view>
		
	
</template>

<script>
	import cmdProgress from "@/components/cmd-progress/cmd-progress.vue"
	import uniCard from "@/components/uni-card/uni-card.vue"
	export default {
		data() {
			return {
				name: '某某某',
				width: 0,
				tips: '您和您的对象的亲密度还有提升的空间，要继续加油打卡、完成flag哦',
				percent:80,
				partnerHead:"../../static/logo.png"
			}
		},
		onNavigationBarButtonTap(e) {
		    console.log("success");
			uni.navigateTo({
				url:'../partner/editPartner'
			});
		},
		components: {
			cmdProgress,
			uniCard
		},
		onLoad() {
		    console.log("111")
		},
		onShow(){
			console.log("222");     //这里写初始化
			// uni.request({
			// 	url: '',
			// 	method: "GET",
			// 	success: function(res) {
			// 		console.log(res.data);
			// 	}
			// });
		},
		methods: {
			detail(){
				uni.navigateTo({
					url: '../partner/intimacy'
				});
			}
		}
	}
</script>

<style>
.qinmidu{
		/* 定义flex容器 */
		display: flex;
         /*设置容器内部容器的排列方向*/	
		flex-direction: row; 
		margin-top: 10%;
		text-align: center;
		margin-left: 10%;
	}
	.avatar {
		height: 150px;
		width: 150px;
		border-radius: 50%;
		-webkit-border-radius: 50%;
		-moz-border-radius: 50%;
		margin-top: 15%;
	}
	.name {
		margin-top: 10%;
	}
	.tips{
		display: flex;
		flex-direction: row; 
		align-items: center;
		justify-content: center;
		margin-top: 10%;
		margin-left: 10%;
		margin-right: 10%;
		/* width: 90%; */
	}
	
</style>
