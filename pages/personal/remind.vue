<template>
	<view>
		<view>
			<uni-nav-bar left-icon="back" left-text="返回" right-text="保存" title="打卡提醒" @clickLeft="back()"></uni-nav-bar>
		</view>
		<view class="text-view">
			主Flag提醒
			<evan-switch class="switch" @change="onChagne1" v-model="checked1"></evan-switch>
		</view>

		<view class="text-view" @tap="showPicker('time1')">
			提醒时间
			<image class="enter-icon" src="../../static/image/向右.png"></image>
			<text class="time-text">{{time1}}</text>
		</view>
		<w-picker mode="t" value="18:08:05" :current="true" @confirm="onConfirm1($event,'time1')" ref="time1"></w-picker>

		<view class="text-view" @tap="showPop('cycle1')">
			重复周期
			<image class="enter-icon" src="../../static/image/向右.png"></image>
			<text class="time-text">{{cycle1}}</text>
		</view>
		<uni-popup ref="cycle1" type="bottom">
			<view class="pop-view">
				<view class="pop-text">重复周期</view>
				<button class="pop-btn" @click="chooseCycle1('仅一次')">仅一次</button>
				<button class="pop-btn" @click="chooseCycle1('每天')">每天</button>
				<button class="pop-btn" @click="chooseCycle1('周一至周五')">周一至周五</button>
			</view>
		</uni-popup>

		<view class="text-view">
			消灭Flag提醒
			<evan-switch class="switch" @change="onChagne2" v-model="checked2"></evan-switch>
		</view>
		
		<view class="text-view" @tap="showPicker('time2')">
			提醒时间
			<image class="enter-icon" src="../../static/image/向右.png"></image>
			<text class="time-text">{{time2}}</text> 
		</view>
		<w-picker mode="time" value="18:08:05" :current="true" @confirm="onConfirm2($event,'time2')" ref="time2"></w-picker>
		
		<view class="text-view" @tap="showPop('cycle2')">
			重复周期
			<image class="enter-icon" src="../../static/image/向右.png"></image>
			<text class="time-text">{{cycle2}}</text>
		</view>
		<uni-popup ref="cycle2" type="bottom">
			<view class="pop-view">
				<view class="pop-text">重复周期</view>
				<button class="pop-btn" @click="chooseCycle2('仅一次')">仅一次</button>
				<button class="pop-btn" @click="chooseCycle2('每天')">每天</button>
				<button class="pop-btn" @click="chooseCycle2('周一至周五')">周一至周五</button>
			</view>
		</uni-popup>
	</view>
</template>

<script>
	import uniNavBar from "@/components/uni-nav-bar/uni-nav-bar.vue"
	import EvanSwitch from "@/components/evan-switch/evan-switch.vue"
	import wPicker from "@/components/w-picker/w-picker.vue";
	import uniPopup from "@/components/uni-popup/uni-popup.vue"

	export default {
		data() {
			return {
				checked1: true,
				time1: '08:00:00',
				cycle1: '每天',
				checked2:true,
				time2: '20:00:00',
				cycle2: '每天',
			}
		},
		components: {
			uniNavBar,
			EvanSwitch,
			wPicker,
			uniPopup
		},
		methods: {
			back() {
				uni.navigateBack({
					delta: 1
				});
			},
			onChagne1(e) {
				console.log(e)
			},
			onChagne2(e) {
				console.log(e)
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
			},
			onConfirm1(res, type) {
				this.time1 = res.result;
			},
			onConfirm2(res, type) {
				this.time2 = res.result;
			},
			chooseCycle1(msg) {
				this.cycle1 = msg;
			},
			chooseCycle2(msg) {
				this.cycle2 = msg;
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

	.switch {
		float: right;
		margin-top: -6px;
	}

	.enter-icon {
		height: 25px;
		width: 25px;
		float: right;
		margin-top: -2px;
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
