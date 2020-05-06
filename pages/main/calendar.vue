<template>
	<view>
		<view>
			<!-- 插入模式 -->
			<uni-calendar :selected="info.selected" :showMonth="false" @change="change" @monthSwitch="monthSwitch" />
		</view>
		<uni-section title="今日" type="line"></uni-section>
		<checkbox-group class="uni-list" @change="checkboxChange">
		    <label class="uni-list-cell uni-list-cell-pd" v-for="item in checkboxItems" :key="item.name">
		        <view class="label-view">
		            <checkbox :value="item.name" :checked="item.checked"></checkbox>
					{{item.value}}
					<!-- <button type="default" plain="true" size="mini">按钮</button> -->
		        </view>
		        <!-- <view>{{item.value}}</view> -->
		    </label>
		</checkbox-group>
	</view>
</template>

<script>
	import uniCalendar from '@/components/uni-calendar/uni-calendar.vue'
	import uniSection from '@/components/uni-section/uni-section.vue'
	export default {
		components: {
			uniCalendar,
			uniSection
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
			var that = this;
			//接口
			uni.getStorage({
				key: 'email',
				success: function(res) {
					console.log('这是key中的内容：' + res.data)
					uni.request({
						url: 'http://iflag.icube.fun:8080/onetime/findByDate/' + res.data + '/' + that.currentDate,
						method: "GET",
						sslVerify: false,
						success: function(response) {
							console.log(response)
							console.log("试一试")
							console.log(that.currentDate)
						},
						fail: function(response) {
							console.log(response.data);
						}
					});
					
					uni.request({
						url: 'http://iflag.icube.fun:8080/periodic/findByDate/' + res.data + '/' + that.currentDate,
						method: "GET",
						sslVerify: false,
						success: function(response) {
							console.log(response)
							console.log("这是周期性任务")
							console.log(that.currentDate)
						},
						fail: function(response) {
							console.log(response.data);
						}
					});
				}
			})
		},
		
		methods:{
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
				
				var that = this;
				uni.getStorage({
					key: 'email',
					success: function(res){
						console.log('这是key中的内容：' + res.data)
						uni.request({
							url: 'http://iflag.icube.fun:8080/onetime/findByDate/' + res.data + '/' + that.selectedDate,
							method: "GET",
							sslVerify: false,
							success: function(response) {
								console.log(response)
								console.log("显示选择当天的一次性flag")
								console.log(that.selectedDate)
							},
							fail: function(response) {
								console.log(response.data);
							}
						});
						console.log('这是key中的内容：' + res.data)
						uni.request({
							url: 'http://iflag.icube.fun:8080/onetime/findByDate/' + res.data + '/' + that.selectedDate,
							method: "GET",
							sslVerify: false,
							success: function(response) {
								console.log(response)
								console.log("显示选择当天的周期性flag")
								console.log(that.selectedDate)
							},
							fail: function(response) {
								console.log(response.data);
							}
						});
					}
				})
				
				
			},
			monthSwitch(e) {
				console.log('monthSwitchs 返回:', e)
			},
			checkboxChange: function(e) {
			    console.log(e)
			    var checked = e.target.value
			    var changed = {}
			    for (var i = 0; i < this.checkboxItems.length; i++) {
			        if (checked.indexOf(this.checkboxItems[i].name) !== -1) {
			            changed['checkboxItems[' + i + '].checked'] = true
			        } else {
			            changed['checkboxItems[' + i + '].checked'] = false
			        }
			    }
			},
		}
	}
</script>

<style>
	.label-view{
		border-bottom: 1px solid lightgray;
		border-radius: 0;
		height: 35px;
	}
</style>
