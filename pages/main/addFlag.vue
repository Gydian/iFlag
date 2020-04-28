<template>
	<view>
		<uni-nav-bar left-icon="back" left-text="返回" right-text="保存" title="一次性任务" @clickLeft="back()"></uni-nav-bar>
		<view class="text-view">
		    <input class="uni-input" focus placeholder="请输入FLAG内容" />
		</view>
		<view class="text-view">
			截止时间
			<image class="enter-icon" src="../../static/image/向右.png"></image>
			<!-- <picker mode="date" :value="date" :start="startDate" :end="endDate" @change="bindDateChange">
			    <view class="enddate-view">{{date}}</view>
			</picker> -->
			<KXDateTime class="enddate-view" :data='date' :end='endDate' :start='startDate' @rundata='kxdatetime'></KXDateTime>
		</view>
		<view class="text-view">
			是否提醒
			<evan-switch class="switch" @change="onChagne2" v-model="checked2"></evan-switch>
		</view>
		
		<view v-if="viewre == true" class="text-view" @tap="showPicker('time1')">
			提醒时间
			<image class="enter-icon" src="../../static/image/向右.png"></image>
			<text class="time-text">{{time1}}</text>
		</view>
		<w-picker mode="t" value="18:08:05" :current="true" @confirm="onConfirm1($event,'time1')" ref="time1"></w-picker>
		
		<view class="text-view">
			多次提醒
			<evan-switch class="switch" @change="onChagne1" v-model="checked1"></evan-switch>
		</view>
		
		<view v-if="viewvi == true" class="text-view"  @tap="showPop('cycle1')" id="alarmInterval">
			提醒间隔
			<image class="enter-icon" src="../../static/image/向右.png"></image>
			<text class="time-text">{{cycle1}}</text>
		</view>
		<uni-popup ref="cycle1" type="bottom">
			<view class="pop-view">
				<view class="pop-text">提醒间隔</view>
				<button class="pop-btn" @click="chooseCycle1('仅一次')">仅一次</button>
				<button class="pop-btn" @click="chooseCycle1('每天')">每天</button>
				<button class="pop-btn" @click="chooseCycle1('周一至周五')">周一至周五</button>
			</view>
		</uni-popup>
	</view>
</template>

<script>
	import uniNavBar from "@/components/uni-nav-bar/uni-nav-bar.vue"
	import EvanSwitch from "@/components/evan-switch/evan-switch.vue"
	import wPicker from "@/components/w-picker/w-picker.vue";
	import uniPopup from "@/components/uni-popup/uni-popup.vue"
	import KXDateTime from "@/components/kx-datetime/kx-datetime.vue"
	
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
				checked2: false,
				time1: '08:00:00',
				cycle1: '每天',
				viewvi: false,	// 是否显示提醒间隔
				viewre: false,	// 是否提醒
			}
		},
		components: {
			uniNavBar,
			EvanSwitch,
			wPicker,
			uniPopup,
			KXDateTime
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
			kxdatetime(e){
			    this.date=e
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
			        year = year + 10;
			    }
			    month = month > 9 ? month : '0' + month;;
			    day = day > 9 ? day : '0' + day;
			    return `${year}-${month}-${day}`;
			},
			onChagne1(e) {
				console.log(e)
				if(this.checked1 == true){
					this.viewvi = true;
					console.log("viewvi = true");
				}
				else if(this.checked1 == false){
					this.viewvi = false;
					console.log("viewvi = false");
				}
			},
			onChagne2(e) {
				console.log(e)
				if(this.checked2 == true){
					this.viewre = true;
					console.log("viewre = true");
				}
				else if(this.checked2 == false){
					this.viewre = false;
					console.log("viewre = false");
				}
			},
			onConfirm1(res, type) {
				this.time1 = res.result;
			},
			chooseCycle1(msg) {
				this.cycle1 = msg;
			},
			showPicker(type) {
				this.$refs[type].show();
			},
			showPop(type) {
				if (type == 'cycle1') {
					this.$refs.cycle1.open()
				} else if (type == 'cycle2') {
					this.$refs.cycle2.open()
				}
			}
			
			
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
	.text-view-hide {
		border-bottom: 1px solid lightgray;
		border-radius: 0;
		height: 18px;
		background-color: #fff;
		text-align: left;
		padding: 20px;
		font-size: 18px;
		/* display: none; */
	}
	.enddate-view {
		float: right;
		margin-right: 2%;
		margin-top: 0%;
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
