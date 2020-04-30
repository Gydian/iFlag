<template>
	<view style="text-align: center;">
		<image class="avatar" :src="this.partnerHead"></image>
	<!-- 	<image class="avatar"></image> -->
		<view class="name">昵称：{{this.name}}</view>
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
				token:'',
				name: '请右上角编辑昵称',
				width: 0,
				tips: '请右上角编辑对象',
				percent:0,
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
			console.log("onshow")
			this.init();
		},
		methods: {
			detail(){
				uni.navigateTo({
					url: '../partner/intimacy'
				});
			},
			init(){
				var that = this;
				uni.getStorage({
					key:'email',
					success:function(res){
						// console.log('这是key中的内容：'+res.data.token)
						that.token = res.data.token;
						// that.partnerHead = "http://59.110.64.233:8080/user/object/"+that.token;
						uni.request({
							url: 'http://iflag.icube.fun:8080/ObjectCenter/'+res.data.token,
							method: "GET",
							sslVerify:false,
							success: function(response) {
								console.log(response)
								if(response.data.StatusCode==0){
									that.name = response.data.Messenger.nickname;
									that.percent = response.data.Messenger.intimateNumber;
									if(that.percent==100){
										that.tips = "您的亲密度很高，要继续加油打卡、完成flag哦!";
									}
									if(that.percent<100&&that.percent>=60){
										that.tips = "您和您的对象的亲密度还有提升的空间，要继续加油打卡、完成flag哦!";
									}
									if(that.percent<60){
										that.tips = "您和您的对象的亲密度太低了，要坚持打卡、完成flag哦!";
									}
								}
								else if(response.data.StatusCode==-13){
									uni.showModal({
										content: "请右上角编辑对象",
										showCancel:false,
										// 这个注释别删
										// success: function (res) {
										//         if (res.confirm) {
										//             uni.navigateTo({
										//             	url: '../partner/editPartner'
										//             });
										//         }
										//     }
									})
								}
								else{
									uni.showModal({
										content: response.data.Messenger,
										showCancel:false,
									})
								}
							},
							fail: function(response) {
								// console.log(response.data);
							}
						});
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
								// console.log(response.data);
							}
						});
					}
				})
			}
		}
	}
	function getRandom(start, end, fixed=0) {
	            let differ = end - start
	            let random = Math.random()
	            return (start + differ * random).toFixed(fixed)
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
