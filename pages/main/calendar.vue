<template>
	<view>
		<view>
			<!-- 插入模式 -->
			<uni-calendar :selected="info.selected" :showMonth="false" @change="change" @monthSwitch="monthSwitch" />
		</view>


		<view class="index" v-bind:style="[{'min-height': secondHeight + 'px' }]">
			<uni-section title="一次性任务" type="line" style="height: 30px;"></uni-section>
			<!-- 数据列表 -->
			<view class="list-box">
	<!-- 			<view style="margin-top: 90upx;"></view> -->
	<checkbox-group class="uni-list" @change="checkboxChange1">

		
				<view class="container_of_slide" v-for="(item,index) in list1" :key="index">
					<view class="slide_list" @touchstart="touchStart1($event,index)" @touchend="touchEnd1($event,index)" @touchmove="touchMove1($event,index)"
					 @tap="recover1(index)" :style="{transform:'translate3d('+item.slide_x+'px, 0, 0)'}">
						<view class="now-message-info" hover-class="uni-list-cell-hover" :style="{width:Screen_width+'px'}" @click="getDetail(item)">
							<checkbox :value="item.id" :checked="item.finish"></checkbox>
						
							<view class="list-right">
								<view class="list-title" v-if="item.finish==true" style="color: #D3D3D3;">{{item.content}}</view>
								<view class="list-title" v-else="item.finish==false">{{item.content}}</view>
							</view>
							
						</view>

						<view class="group-btn">
							<view class="top btn-div" @tap="top1(item.id)">
								编辑
							</view>
							<view class="removeM btn-div" @tap="removeM1(index, item.id)">
								删除
							</view>
						</view>
						<view style="clear:both"></view>
					</view>
				</view>
				</checkbox-group>
			</view>
			<uni-section title="长期任务" type="line" style="height: 30px;"></uni-section>
			<view class="list-box">
			<!-- 			<view style="margin-top: 90upx;"></view> -->
			<checkbox-group class="uni-list" @change="checkboxChange2">
				
						<view class="container_of_slide" v-for="(item,index) in list2" :key="index">
							<view class="slide_list" @touchstart="touchStart2($event,index)" @touchend="touchEnd2($event,index)" @touchmove="touchMove2($event,index)"
							 @tap="recover2(index)" :style="{transform:'translate3d('+item.slide_x+'px, 0, 0)'}">
								<view class="now-message-info" hover-class="uni-list-cell-hover" :style="{width:Screen_width+'px'}" @click="getDetail(item)">
									<checkbox :value="item.id" :checked="item.finish"></checkbox>
								
									<view class="list-right">
										<view class="list-title" v-if="item.finish==true" style="color: #D3D3D3;">{{item.content}}</view>
										<view class="list-title" v-else="item.finish==false">{{item.content}}</view>
									</view>
									
								</view>
								<view class="group-btn">
									<view class="top btn-div" @tap="top2(item.id)">
										编辑
									</view>
									<view class="removeM btn-div" @tap="removeM2(index, item.id)">
										删除
									</view>
								</view>
								<view style="clear:both"></view>
							</view>
						</view>
						</checkbox-group>
					</view>

			
		</view>
	</view>
</template>

<script>
	import uniCalendar from '@/components/uni-calendar/uni-calendar.vue'
	import uniSection from '@/components/uni-section/uni-section.vue'
	import mSearch from '@/components/m-search/m-search.vue'
	export default {
		components: {
			uniCalendar,
			uniSection,
			mSearch
		},
		computed: {
			Screen_width() {
				return uni.getSystemInfoSync().windowWidth;
			}
		},
		data() {
			return{
				showCalendar: false,
				info: {
					date: '2019-08-15',
					insert: false,
					selected: []
				},
				checkboxItems: [
					{
						name: 'flag1',
						value: '写作业'
				    },
				    {
				        name: 'flag2',
				        value: '预习课本',
				        // checked: 'true'
				    }
				],
				currentDate: new Date().toISOString().slice(0, 10),
				selectedDate: '',
				
				img: '../../static/slide-list/qr_code.png',
				visible: false,
				start_slide_x: 0,
				btnWidth: 0,
				startX: 0,
				LastX: 0,
				startTime: 0,
				screenName: '',
				slide_x:0,
				list1 : [
					{
						id: 1,
						surname: '张',
						name: '张三',
						dateTime: '2019-03-18',
						remarks: 'XXXXXXXXXXXXXXXXXXX公司',
						isShare: true,
						slide_x: 0
					},
					{
						id: 2,
						surname: '?',
						name: '李二',
						dateTime: '2019-03-17',
						remarks: 'XXXXXXXXXXXXXXXXXXX公司',
						isShare: false,
						slide_x: 0
					},
					{
						id: 3,
						surname: '王',
						name: '王五',
						dateTime: '2019-03-18',
						remarks: 'XXXXXXXXXXXXXXXXXXX公司',
						isShare: true,
						slide_x: 0
					},
					{
						id: 4,
						surname: '李',
						name: '李珊珊',
						dateTime: '2019-03-18',
						remarks: 'XXXXXXXXXXXXXXXXXXX公司',
						isShare: true,
						slide_x: 0
					}
				],
				list2 : [
					{
						id: 1,
						surname: '张',
						name: '张三',
						dateTime: '2019-03-18',
						remarks: 'XXXXXXXXXXXXXXXXXXX公司',
						isShare: true,
						slide_x: 0
					},
					{
						id: 2,
						surname: '?',
						name: '李二',
						dateTime: '2019-03-17',
						remarks: 'XXXXXXXXXXXXXXXXXXX公司',
						isShare: false,
						slide_x: 0
					},
					{
						id: 3,
						surname: '王',
						name: '王五',
						dateTime: '2019-03-18',
						remarks: 'XXXXXXXXXXXXXXXXXXX公司',
						isShare: true,
						slide_x: 0
					},
					{
						id: 4,
						surname: '李',
						name: '李珊珊',
						dateTime: '2019-03-18',
						remarks: 'XXXXXXXXXXXXXXXXXXX公司',
						isShare: true,
						slide_x: 0
					}
				],
				btuBottom: '0',
				secondHeight: '',
				// checkList:[],
			}
		},
		onReady() {
			this.$nextTick(() => {
				this.showCalendar = true
			})
			// TODO 模拟请求异步同步数据
			// setTimeout(() => {
			// 	this.info.selected = [{
			// 			date: '2019-08-20',
			// 			info: '打卡'
			// 		},
			// 		{
			// 			date: '2019-08-21',
			// 			info: '签到',
			// 			data: {
			// 				custom: '自定义信息',
			// 				name: '自定义消息头'
			// 			}
			// 		},
			// 		{
			// 			date: '2019-08-22',
			// 			info: '已打卡'
			// 		}
			// 	]
			// }, 500)
		},
		
		onShow: function(){
			this.init(this.currentDate);
		},
		
		methods:{
			init(mydate){
				var that = this;
				//接口
				uni.getStorage({
					key: 'email',

					success: function(res) {
						console.log('这是key中的内容：' + res.data)
						uni.request({
							url: 'http://iflag.icube.fun:8080/onetime/findByDate/'+res.data.userid + '/' + mydate,
							method: "GET",
							sslVerify: false,
							success: function(response) {
								console.log(response)
								console.log("试一试")
								console.log(that.currentDate)
								that.list1 = response.data
								that.list1.sort((a,b)=>{    return a.finish -b.finish
								})
								console.log(that.list1)
								that.list1.forEach((item,index)=>{
									that.$set(item,'slide_x',0)
								})
								console.log(that.list1)

							},
							fail: function(response) {
								console.log(response.data);
							}
						});

						
						uni.request({
							url: 'http://iflag.icube.fun:8080/periodic/findByDate/'+res.data.userid + '/' + mydate,

							method: "GET",
							sslVerify: false,
							success: function(response) {
								console.log(response)
								console.log("这是周期性任务")
								console.log(that.currentDate)
								that.list2 = response.data
								that.list2.sort((a,b)=>{    return a.finish -b.finish
								})
								that.list2.forEach((item,index)=>{
									that.$set(item,'slide_x',0)
								})
							},
							fail: function(response) {
								console.log(response.data);
							}
						});
					}
				})
			},
			change(e) {			// 获取当日flag内容
				console.log('change 返回:', e)
				// 模拟动态打卡
				//if (this.info.selected.length > 5) return
				this.info.selected.push({
					date: e.fulldate,
					info: '',
					//selectedDate = e.fulldate
					
				})
				this.selectedDate = e.fulldate
				this.init(this.selectedDate)	
			},
			monthSwitch(e) {
				console.log('monthSwitchs 返回:', e)
			},
			checkboxChange1: function(e) {
				var items = this.list1; 
				var checked = e.target.value
				for (var i = 0, lenI = this.list1.length; i < lenI; ++i) {
					const item = this.list1[i]
					if(checked.indexOf(this.list1[i].id) !== -1){
						this.$set(item,'finish',true)
						console.log("111");
					}else{
						this.$set(item,'finish',false)
					}
					console.log(this.list1[i]);
					uni.request({
						url: 'http://iflag.icube.fun:8080/onetime/update',
						dataType:"JSON",
						data: this.list1[i],
						method: "PUT",
						header: {
							"Content-Type": "application/json"
						},
						
						sslVerify: false,
						success: function(res) {
							console.log(res.data)
						},
						fail: function(res) {
							console.log(res.data);
						}
					});
				}
			},
			checkboxChange2: function(e) {
				var items = this.list2;
				var checked = e.target.value
				for (var i = 0, lenI = this.list2.length; i < lenI; ++i) {
					const item = this.list2[i]
					if(checked.indexOf(this.list2[i].id) !== -1){
						this.$set(item,'finish',true)
						console.log("222");
					}else{
						this.$set(item,'finish',false)
					}
					console.log(this.list2[i]);
					uni.request({
						url: 'http://iflag.icube.fun:8080/periodic/update',
						dataType:"JSON",
						data: this.list2[i],
						method: "PUT",
						header: {
							"Content-Type": "application/json"
						},
						
						sslVerify: false,
						success: function(res) {
							console.log(res.data)
						},
						fail: function(res) {
							console.log(res.data);
						}
					});
				}
			    // console.log(e.target.value)
			    // var checked = e.target.value
			    // var changed = {}
			    // for (var i = 0; i < this.list2.length; i++) {
			    //     if (checked.indexOf(this.list2[i].name) !== -1) {
			    //         changed['list2[' + i + '].checked'] = true
			    //     } else {
			    //         changed['list2[' + i + '].checked'] = false
			    //     }
			    // }
			},
			
			cancelEvent(){
					this.visible = false
			},
			search(e, val) {
				this.screenName = e
				console.log('点击搜索')
			},
			addCustomer(){
				console.log('点击添加按钮')
			},
			getDetail(item){
				console.log('查看详情')
			},
			// 滑动开始
			touchStart1(e, index) {
				//记录手指放上去的时间
				this.startTime = e.timeStamp;
				//记录滑块的初始位置
				this.start_slide_x = this.list1[index].slide_x;
				// 按钮宽度
				uni.createSelectorQuery()
					.selectAll('.group-btn')
					.boundingClientRect()
					.exec(res => {
						if (res[0] != null) {
							this.btnWidth = res[0][index].width * -1;
						}
					});
				// 记录上一次开始时手指所处位置
				this.startX = e.touches[0].pageX;
				// 记录上一次手指位置
				this.lastX = this.startX;
				//初始化非当前滑动消息列的位置
				this.list1.forEach((item, eq) => {
					if (eq !== index) {
						item.slide_x = 0;
					}
				});
			},
			touchStart2(e, index) {
				//记录手指放上去的时间
				this.startTime = e.timeStamp;
				//记录滑块的初始位置
				this.start_slide_x = this.list2[index].slide_x;
				// 按钮宽度
				uni.createSelectorQuery()
					.selectAll('.group-btn')
					.boundingClientRect()
					.exec(res => {
						if (res[0] != null) {
							this.btnWidth = res[0][index].width * -1;
						}
					});
				// 记录上一次开始时手指所处位置
				this.startX = e.touches[0].pageX;
				// 记录上一次手指位置
				this.lastX = this.startX;
				//初始化非当前滑动消息列的位置
				this.list2.forEach((item, eq) => {
					if (eq !== index) {
						item.slide_x = 0;
					}
				});
			},
			// 滑动中
			touchMove1(e, index) {
				const endX = e.touches[0].pageX;
				const distance = endX - this.lastX;
				// 预测滑块所处位置
				const duang = this.list1[index].slide_x + distance;
				// 如果在可行区域内
				if (duang <= 0 && duang >= this.btnWidth) {
					this.list1[index].slide_x = duang;
				}
				// 此处手指所处位置将成为下次手指移动时的上一次位置
				this.lastX = endX;
			},
			touchMove2(e, index) {
				const endX = e.touches[0].pageX;
				const distance = endX - this.lastX;
				// 预测滑块所处位置
				const duang = this.list2[index].slide_x + distance;
				// 如果在可行区域内
				if (duang <= 0 && duang >= this.btnWidth) {
					this.list2[index].slide_x = duang;
				}
				// 此处手指所处位置将成为下次手指移动时的上一次位置
				this.lastX = endX;
			},
			// 滑动结束
			touchEnd1(e, index) {
				let distance = 10;
				const endTime = e.timeStamp;
				const x_end_distance = this.startX - this.lastX;
				if (Math.abs(endTime - this.startTime) > 200) {
					distance = this.btnWidth / -2;
				}
				// 判断手指最终位置与手指开始位置的位置差距
				if (x_end_distance > distance) {
					this.list1[index].slide_x = this.btnWidth;
				} else if (x_end_distance < distance * -1) {
					this.list1[index].slide_x = 0;
				} else {
					this.list1[index].slide_x = this.start_slide_x;
				}
			},
			touchEnd2(e, index) {
				let distance = 10;
				const endTime = e.timeStamp;
				const x_end_distance = this.startX - this.lastX;
				if (Math.abs(endTime - this.startTime) > 200) {
					distance = this.btnWidth / -2;
				}
				// 判断手指最终位置与手指开始位置的位置差距
				if (x_end_distance > distance) {
					this.list2[index].slide_x = this.btnWidth;
				} else if (x_end_distance < distance * -1) {
					this.list2[index].slide_x = 0;
				} else {
					this.list2[index].slide_x = this.start_slide_x;
				}
			},
			// 点击回复原状
			recover1(index) {
				this.list1[index].slide_x = 0;
			},
			recover2(index) {
				this.list2[index].slide_x = 0;
			},
			// 编辑
			top1(id) {
				uni.redirectTo({
					url: './addFlag?id=11&flagid='+id
				});
			},
			top2(id) {
				console.log(id)
				uni.redirectTo({
					url: './addPerFlag?id=11&flagid='+id
				});
			},
			// 删除
			removeM1(index, id) {
				console.log(id)
				let self = this
				console.log('点击删除')
				uni.showModal({
					title: '',
					content: '确定要删除该信息吗？',
					confirmText: '删除',
					confirmColor: '#ff3b32',
					success: function (res) {
						if (res.confirm) {
							console.log('用户点击确定')
							uni.request({
								url: 'http://iflag.icube.fun:8080/onetime/deleteById/'+id,
							
								method: "DELETE",
								sslVerify: false,
								success: function(response) {
									console.log(response.data)
									if(response.data){
										self.list1.splice(index, 1)
										uni.showToast({
											icon: "success",
											title: '操作成功!',
											duration: 2000
										});
									}
									else{
										uni.showModal({
											content: "删除失败",
											showCancel:false
										})
									}
								},
								fail: function(response) {
									console.log(response.data);
								}
							});
							
						} else if (res.cancel) {
							console.log('用户点击取消')
						}
					}
				});
			},
			removeM2(index, id) {
				let self = this
				console.log('点击删除')
				uni.showModal({
					title: '',
					content: '确定要删除该信息吗？',
					confirmText: '删除',
					confirmColor: '#ff3b32',
					success: function (res) {
						if (res.confirm) {
							console.log('用户点击确定')
							uni.request({
								url: 'http://iflag.icube.fun:8080/periodic/deleteById/'+id,
							
								method: "DELETE",
								sslVerify: false,
								success: function(response) {
									console.log(response.data)
									if(response.data){
										self.list2.splice(index, 1)
										uni.showToast({
											icon: "success",
											title: '操作成功!',
											duration: 2000
										});
									}
									else{
										uni.showModal({
											content: "删除失败",
											showCancel:false
										})
									}
								},
								fail: function(response) {
									console.log(response.data);
								}
							});
							
						} else if (res.cancel) {
							console.log('用户点击取消')
						}
					}
				});
			}
		}		
	}
</script>

<style scoped>
	.label-view{
		border-bottom: 1px solid lightgray;
		border-radius: 0;
		height: 15px;
	}
	
	.index{
		background: #F8F8F8;
	}
	.list-box{
		padding: 20upx 0;
	}
	.container_of_slide {
		width: 100%;
		overflow: hidden;
	}
	
	.slide_list {
		transition: all 100ms;
		transition-timing-function: ease-out;
		min-width: 200%;
		height: 100upx;
	}
	
	.now-message-info {
		box-sizing:border-box;
		display: flex;
		align-items: center;
		/* justify-content: space-between; */
		font-size: 16px;
		clear:both;
		height: 100upx;
		padding: 0 30upx;
		margin-bottom: 20upx;
		background: #FFFFFF;
	}
	.now-message-info,
	.group-btn {
		float: left;
	}
	
	.group-btn {
		display: flex;
		flex-direction: row;
		height: 100upx;
		min-width: 100upx;
		align-items: center;
	
	}
	
	.group-btn .btn-div {
		height: 100upx;
		color: #fff;
		text-align: center;
		padding: 0 50upx;
		font-size: 34upx;
		line-height: 100upx;
	}
	
	.group-btn .top {
		background-color: #c4c7cd;
	}
	
	.group-btn .removeM {
		background-color: #ff3b32;
	}
	
	
	.icon-circle{
		background: #3396fb;
		border-radius: 100%;
		width:100upx;
		height: 100upx;
		line-height:100upx;
		text-align:center;
		color: #FFFFFF;
		font-weight: bold;
		font-size: 20px;
		float: left;
	}
	.list-right{
		float: left;
		margin-left: 25upx;
		margin-right: 30upx;
	}
	.list-right-1{
		float: right;
		color: #A9A9A9;
	}
	.list-title{
		width: 350upx;
		line-height:1.5;
		overflow:hidden;
		margin-bottom: 0upx;
		color:#333;
		display:-webkit-box;
		-webkit-box-orient:vertical;
		-webkit-line-clamp:1;
		overflow:hidden;
	}
	.list-detail{
		width: 350upx;
		font-size: 14px;
		color: #a9a9a9;
		display:-webkit-box;
		-webkit-box-orient:vertical;
		-webkit-line-clamp:1;
		overflow:hidden;
	}
	.button-box{
		box-sizing: border-box;
		position: fixed;
		left: 0;
		bottom: 0;
		width: 100%;
		z-index: 998;
		background: #F8F8F8;
	}
	.btn-sub{
		display: -webkit-box;
		display: -webkit-flex;
		display: flex;
		-webkit-box-pack: center;
		-webkit-justify-content: center;
		justify-content: center;
		-webkit-box-align: center;
		-webkit-align-items: center;
		align-items: center;
		-webkit-box-orient: vertical;
		-webkit-box-direction: normal;
		float: left;
		width: 100%;
		height: 100upx;
		background: #F8F8F8;
		color: #7fb2ff;
	}
	.empty{
		color: #999999;
	}
	.plusempty-img{
		width: 50upx;
		height: 50upx;
		margin-top: 30upx;
	}
.uni-section{
	margin-top: 0px;
}




	.uni-list-cell-hover {
		background-color: #eeeeee;
	}
	.bottom-btn-hover{
		background: #0564c7!important;;
	}
</style>
