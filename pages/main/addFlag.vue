<template>
	<view>
		<uni-nav-bar left-icon="back" left-text="返回" right-text="保存" title="一次性任务" @clickLeft="back()"></uni-nav-bar>
		<view class="text-view">
		    <input class="uni-input" focus placeholder="请输入FLAG内容" />
		</view>
		<view class="text-view">
			截止时间
			<image class="enter-icon" src="../../static/image/向右.png"></image>
			<picker mode="date" :value="date" :start="startDate" :end="endDate" @change="bindDateChange">
			    <view class="enddate-view">{{date}}</view>
			</picker>
		</view>
		<view class="text-view" @tap="showPicker('time1')">
			提醒时间
			<image class="enter-icon" src="../../static/image/向右.png"></image>
			<text class="time-text">{{time1}}</text>
		</view>
		<w-picker mode="t" value="18:08:05" :current="true" @confirm="onConfirm1($event,'time1')" ref="time1"></w-picker>
		
		<view class="text-view">
			多次提醒
			<evan-switch class="switch" @change="onChagne1" v-model="checked1"></evan-switch>
		</view>
	</view>
</template>

<script>
	import uniNavBar from "@/components/uni-nav-bar/uni-nav-bar.vue"
	import EvanSwitch from "@/components/evan-switch/evan-switch.vue"
	import wPicker from "@/components/w-picker/w-picker.vue";
	import uniPopup from "@/components/uni-popup/uni-popup.vue"
	
	export default{
		data(){
			const currentDate = this.getDate({
			    format: true
			})
			return{
				title: 'one-time flag',
				focus: false,
				inputValue: '',
				changeValue: '',
				date: currentDate,
				checked1: false,
				time1: '08:00:00',
			}
		},
		components: {
			uniNavBar,
			EvanSwitch,
			wPicker,
			uniPopup
		},
		computed: {
		    startDate() {
		        return this.getDate('start');
		    },
		    endDate() {
		        return this.getDate('end');
		    }
		},
		methods:{
			back() {
				uni.navigateBack({
					delta: 1
				});
			},
			bindDateChange: function(e) {
			    this.date = e.target.value
			},
			getDate(type) {
			    const date = new Date();
			    let year = date.getFullYear();
			    let month = date.getMonth() + 1;
			    let day = date.getDate();
			
			    if (type === 'start') {
			        year = year - 60;
			    } else if (type === 'end') {
			        year = year + 2;
			    }
			    month = month > 9 ? month : '0' + month;;
			    day = day > 9 ? day : '0' + day;
			    return `${year}-${month}-${day}`;
			},
			onChagne1(e) {
				console.log(e)
			},
			onConfirm1(res, type) {
				this.time1 = res.result;
			},
			showPicker(type) {
				this.$refs[type].show();
			},
			
		}
	}
</script>

<style>
	.text-view {
		border-bottom: 1px solid lightgray;
		border-radius: 0;
		height: 18px;
		background-color: #fff;
		text-align: left;
		padding: 20px;
		font-size: 18px;
	}
	.enddate-view {
		float: right;
		margin-right: 2%;
		margin-top: -6%;
	}
	.enter-icon {
		height: 25px;
		width: 25px;
		float: right;
		margin-top: -2px;
	}
	.switch {
		float: right;
		margin-top: -6px;
	}
	.time-text {
		float: right;
		margin-right: 2%;
	}
	.pop-view {
		height: 200px;
		background-color: #fff;
		text-align: center;
	}
	.pop-text {
		margin: 5%;
		padding-top: 5%;
		font-size: 18px;
	}
	.pop-btn {
		text-align: left;
		background-color: #fff;
		margin-left: 14px;
		margin-right: 14px;
		border-radius: 0;
		border-top: 1px solid lightgray;
	}
</style>
