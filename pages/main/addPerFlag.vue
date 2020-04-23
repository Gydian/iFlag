<template>
	<view>
		<uni-nav-bar left-icon="back" left-text="返回" right-text="保存" title="周期性任务" @clickLeft="back()"></uni-nav-bar>
		<view class="text-view">
		    <input class="uni-input" focus placeholder="请输入FLAG内容" />
		</view>
		<view class="text-view"  @tap="showPop('cycle1')">
			周期时长
			<image class="enter-icon" src="../../static/image/向右.png"></image>
			<text class="time-text">{{cycle1}}</text>
		</view>
		<uni-popup ref="cycle1" type="bottom">
			<view class="pop-view">
				<view class="pop-text">周期时长</view>
				<button class="pop-btn" @click="chooseCycle1('仅一次')">仅一次</button>
				<button class="pop-btn" @click="chooseCycle1('每天')">每天</button>
				<button class="pop-btn" @click="chooseCycle1('周一至周五')">周一至周五</button>
			</view>
		</uni-popup>
		
		<view class="text-view">
			是否提醒
			<evan-switch class="switch" @change="onChagne1" v-model="checked1"></evan-switch>
		</view>
		<view class="text-view">
			多次提醒
			<evan-switch class="switch" @change="onChagne2" v-model="checked2"></evan-switch>
		</view>
		<view class="text-view">
			是否终止
			<evan-switch class="switch" @change="onChagne3" v-model="checked3"></evan-switch>
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
			return{
				title: 'periodic flag',
				inputValue: '',
				changeValue: '',
				checked1: false,
				checked2: false,
				checked3: false,
				cycle1: '每天',
			}
		},
		components: {
			uniNavBar,
			EvanSwitch,
			wPicker,
			uniPopup
		},
		methods:{
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
			onChagne3(e) {
				console.log(e)
			},
			chooseCycle1(msg) {
				this.cycle1 = msg;
			},
			showPop(type) {
				if (type == 'cycle1') {
					this.$refs.cycle1.open()
				} else if (type == 'cycle2') {
					this.$refs.cycle2.open()
				}
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
	.time-text {
		float: right;
		margin-right: 2%;
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
