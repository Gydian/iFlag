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
				<view v-for="(item, key) in list" :key="item.id">
					<view v-if="item.finish == false">
						<view class="left">
							<view>
								<!-- <image src="../../static/logo.png" style="width:40px;height:40px;border-radius:50%;border:1px solid #aaa;" /> -->
								<!-- <image src="this.partnerHead" style="width:40px;height:40px;border-radius:50%;border:1px solid #aaa;" /> -->
								<image class="avatar" :src="partnerHead"></image>
							</view>
							<view style="margin-left:10px; height:auto">
								<view style="font-size:12px;color:#999;">{{ name }}</view>
								<!-- <view style="font-size:12px;color:#999;">{{ item.finish }}</view> -->
								<view class="langcon">你的任务“{{ item.content }}”还没有完成哟，截止时间是：{{ item.endtime }}</view>
							</view>
						</view>
					</view>
				</view>
				<!-- 注释 -->
				<!-- <view class="zs">点击对方头像可进入私聊模式</view> -->
				<view v-for="(item, key) in list2" :key="item.id">
					<view class="left">
						<view>
							<!-- <image src="../../static/logo.png" style="width:40px;height:40px;border-radius:50%;border:1px solid #aaa;" /> -->
							<image class="avatar" :src="partnerHead"></image>
						</view>
						<view style="margin-left:10px; height:auto">
							<view style="font-size:12px;color:#999;">{{ name }}</view>
							<view class="langcon">你的任务“{{ item.content }}”还没有完成哟，养成好习惯从每天做起！</view>
						</view>
					</view>

				</view>
				<!-- 已完成的 -->
				<view v-for="(item, key) in finishList" :key="item.id">

					<view class="left">
						<view>
							<!-- <image src="../../static/logo.png" style="width:40px;height:40px;border-radius:50%;border:1px solid #aaa;" /> -->
							<image class="avatar" :src="partnerHead"></image>
						</view>
						<view style="margin-left:10px; height:auto">
							<view style="font-size:12px;color:#999;">{{ name }}</view>
							<view class="langcon">恭喜你啦！已经完成了"{{ item.content }}"这一Flag，再接再厉哦！</view>
						</view>
					</view>
				</view>
				
				<view v-for="(item, key) in remindList1">
					<view class="left">
						<view>
							<!-- <image src="../../static/logo.png" style="width:40px;height:40px;border-radius:50%;border:1px solid #aaa;" /> -->
							<image class="avatar" :src="partnerHead"></image>
						</view>
						<view style="margin-left:10px;">
							<view style="font-size:12px;color:#999;">{{ name }}</view>
							<view class="langcon">你的Flag:"{{ item.content }}"还没有完成，快去打卡吧！</view>
						</view>
					</view>
				</view>
				
				<view v-for="(item, key) in remindList2">
					<view class="left">
						<view>
							<!-- <image src="../../static/logo.png" style="width:40px;height:40px;border-radius:50%;border:1px solid #aaa;" /> -->
							<image class="avatar" :src="partnerHead"></image>
						</view>
						<view style="margin-left:10px;">
							<view style="font-size:12px;color:#999;">{{ name }}</view>
							<view class="langcon">有长期任务:"{{ item.content }}"等待完成，快去完成吧！</view>
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
			const currentDate = this.getDate({
			    format: true
			});
			return {
				currentDate: currentDate,
				finishList:[],
				timers1:[],
				timers2:[],
				remindList1:[],
				remindList2:[],
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

				name: '对象',
			}
		},
				
		onShow: function() {
			for(var i = 0;i<this.timers1.length;i++){
				console.log(this.timers1[i]);
				clearInterval(this.timers1[i]);  
			}
			for(var i = 0;i<this.timers2.length;i++){
				console.log(this.timers2[i]);
				clearInterval(this.timers2[i]);  
			}
			console.log("111111111111")
			let app = getApp()
			console.log(app.globalData.finishList)
			this.finishList = app.globalData.finishList
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

					console.log(that.currentDate)
					uni.request({
						
						url: 'http://iflag.icube.fun:8080/remind/'+ res.data.userid+'/' + that.currentDate,
						method: "GET",
						sslVerify: false,
						success: function(response) {
							console.log(response.data)
							var list1 = response.data.onetimeFlagList;
							console.log(list1.length);
							for(var i=0;i<list1.length;i++){
								console.log(list1[i].remindTime + list1[i].content);
								if(list1[i].remindTime!=null){
									console.log(list1[i].remindTime+"不是空");
									var timer = setInterval(function(a) {
										const date = new Date();
										var times = a.remindTime;
										var timearr = times.replace(" ", ":").replace(/\:/g, "-").split("-");
										// console.log(timearr[3]+"时"+date.getHours());
										// console.log(timearr[4]+"分"+date.getMinutes());
										if(timearr[3] == date.getHours()&&timearr[4]==date.getMinutes()){
											console.log("到点了");
											that.remindList1.push(a);
											console.log(that.remindList)
											// that.$set(a,'remind',1)
											clearInterval(timer);
										}
									}, 30000,list1[i]);
									that.timers1.push(timer);
									console.log(that.timers1);
								}	
							}
							
							var list2 = response.data.periodicFlagList;
							console.log(list2.length);
							for(var i=0;i<list2.length;i++){
								console.log(list2[i].remindTime + list2[i].content);
								if(list2[i].remindTime!=null){
									console.log(list2[i].remindTime+"不是空");
									var timer = setInterval(function(a) {
										const date = new Date();
										var times = a.remindTime;
										var timearr = times.replace(" ", ":").replace(/\:/g, "-").split("-");
										// console.log(timearr[3]+"时"+date.getHours());
										// console.log(timearr[4]+"分"+date.getMinutes());
										if(timearr[3] == date.getHours()&&timearr[4]==date.getMinutes()){
											console.log("到点了");
											that.remindList2.push(a);
											console.log(that.remindList)
											clearInterval(timer);
										}
									}, 30000,list2[i]);
									that.timers2.push(timer);
									console.log(that.timers2);
								}	
							}

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
							// console.log(response)
							// console.log("试一试2")
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
					
					// 对象昵称
					uni.request({
						url: 'http://iflag.icube.fun:8080/ObjectCenter/'+res.data.token,
						method: "GET",
						sslVerify:false,
						success: function(response) {
							console.log(response)
							if(response.data.StatusCode==0){
								that.name = response.data.Messenger.nickname;
							}
							else{
								console.log("昵称为对象")
							}
						},
						fail: function(response) {
							// console.log(response.data);
						}
					});
				}
			})
		},
		
		
		methods: {
			getDate(type) {
			    const date = new Date();
			    let year = date.getFullYear();
			    let month = date.getMonth() + 1;
			    let day = date.getDate();
			
			    if (type === 'start') {
			        year = year - 60;
			    } else if (type === 'end') {
			        year = year + 10;
			    }
			    month = month > 9 ? month : '0' + month;;
			    day = day > 9 ? day : '0' + day;
			    return `${year}-${month}-${day}`;
			},
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
		margin-right: 10px;
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
		height: auto;
		line-height: 30px;
		border-radius: 7px;
		margin-top: 5px;
		text-align: left;
		padding: 3px 10px 3px 10px;
		background: #f8f8f8;
	}
</style>

