<template>
	<view>
		<view>
			<!-- 插入模式 -->
			<uni-calendar :selected="info.selected" :showMonth="false" @change="change" @monthSwitch="monthSwitch" />
		</view>
		<uni-section title="今日" type="line"></uni-section>
		
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
				}
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
		methods:{
			change(e) {			// 获取当日flag内容
				console.log('change 返回:', e)
				// 模拟动态打卡
				//if (this.info.selected.length > 5) return
				this.info.selected.push({
					date: e.fulldate,
					info: '未完成'
				})
			},
			monthSwitch(e) {
				console.log('monthSwitchs 返回:', e)
			}
		}
	}
</script>

<style>
</style>
