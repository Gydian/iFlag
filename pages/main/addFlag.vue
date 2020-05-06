<template>
	<view>
		
		<uni-nav-bar left-icon="back" left-text="返回" right-text="保存" title="一次性任务" @clickLeft="back()" @clickRight="save()"></uni-nav-bar>
		<view class="text-view">
		    <input class="uni-input" v-model="inputFlag" focus placeholder="请输入FLAG内容" />
		</view>
		<view class="text-view">
			截止时间
			<image class="enter-icon" src="../../static/image/向右.png"></image>
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
		onLoad: function (option) { //option为object类型，会序列化上个页面传递的参数
		        console.log(option.id); //打印出上个页面传递的参数。
				this.pageid = option.id;
				this.flagid = option.flagid;
				console.log(this.flagid)
		        // console.log(option.name); //打印出上个页面传递的参数。
		    },
		data(){
			const currentDate = this.getDate({
			    format: true
			})
			return{
				flagid : 0,
				pageid:0,
				title: 'one-time flag',
				focus: false,
				inputFlag: '',
				currentDate: currentDate,
				date: currentDate,	// endTime
				checked1: false,
				checked2: false,
				isfinished: false,
				time1: null,
				cycle1: null,
				cycleVal1: null,
				viewvi: false,	// 是否显示提醒间隔
				viewre: false,	// 是否提醒
			}
		},
		onShow: function(){
			if(this.pageid == 11){
				var that = this;
				uni.request({
					url: 'http://iflag.icube.fun:8080/onetime/findById/'+that.flagid,
					method: "GET",
					sslVerify: false,
					success: function(response) {
						that.inputFlag = response.data.content;
						that.date = response.data.endtime;
						that.cycleVal1 = response.data.repeatPeriod;
						that.time1 = response.data.remindTime;
						that.checked1 = response.data.repeat;
						that.checked2 = response.data.remind;
						that.isfinished = response.data.finish;
					},
					fail: function(response) {
						console.log(response.data);
					}
				});
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
			back() {	// 导航栏左侧按钮点击返回上一页
			var that = this;
				console.log(that.pageid);
				if(that.pageid == 11){
					uni.redirectTo({
						url: './calendar'
					});
				}
				else{
					// uni.navigateBack({
					// 	delta: 1
					// });
					uni.switchTab({
						url: '/pages/views/main'
					});
				}
				this.pageid = 0;
				
				
			},
			
			save(){		// 导航栏右侧按钮点击保存flag
				
				console.log("点击保存")
				if(this.inputFlag == '' || this.inputFlag == null){
					uni.showModal({
						content: 'flag内容不能为空！',
						showCancel: false
					})
				}
				else if(this.checked1 == true && this.checked2 == false){
					uni.showModal({
						content: '请选择提醒时间！',
						showCancel: false
					})
				}
				else{
					switch (this.cycle1) {
						case '每天':
							this.cycleVal1 = 'everyday'
							break;
						case '仅一次':
							this.cycleVal1 = 'once'
							break;
						case '周一至周五':
							this.cycleVal1 = 'weekday'
							break;
					}
					var that = this;
					console.log("到这儿了没");
					uni.getStorage({
						key: 'email',
						success: function(res) {
							console.log('这是key中的内容：' + res.data.mail),
							console.log(that.currentDate),
							console.log(that.inputFlag),
							console.log(that.date),
							console.log(that.cycleVal1),
							console.log(that.time1),
							console.log(that.checked1),
							console.log(that.checked2),

							console.log(that.isfinished);
							if(this.pageid = 11){
								uni.request({
									url: 'http://iflag.icube.fun:8080/onetime/update',
									dataType:"JSON",
									data: {
										id: that.flagid,
										userid:res.data.userid,
										date: that.currentDate,
										content: that.inputFlag,
										endtime: that.date,
										repeatPeriod: that.cycleVal1,
										remindTime: that.time1,
										repeat: that.checked1,
										remind: that.checked2,
										finish: that.isfinished
									},
									method: "PUT",
									header: {
										"Content-Type": "application/json"
									},
									
									sslVerify: false,
									success: function(response) {
										console.log(response)
										uni.showModal({
											content: '修改成功！',
											showCancel:false
										})
									},
									fail: function(response) {
										console.log(response.data);
										uni.showModal({
											content: '修改失败，请重试！',
											showCancel:false
										})
									}
								});
							}
							else{
								uni.request({
									
									url: 'http://iflag.icube.fun:8080/onetime/save',
									dataType:"JSON",
									data: {
										userid: res.data.userid,
										date: that.currentDate,
										content: that.inputFlag,
										endtime: that.date,
										repeatPeriod: that.cycleVal1,
										remindTime: that.time1,
										repeat: that.checked1,
										remind: that.checked2,
										finish: that.isfinished
									},
									method: "POST",
									header: {
										"Content-Type": "application/json"
									},
									
									sslVerify: false,
									success: function(response) {
										console.log(response)
										uni.showModal({
											content: '保存成功！',
											showCancel:false
										})
									},
									fail: function(response) {
										console.log(response.data);
										uni.showModal({
											content: '保存失败，请重试！',
											showCancel:false
										})
									}
								});
							}
							
						},
						fail: function(res) {
							console.log(res.data);
							// uni.showModal({
							// 	content: '保存失败，请重试！',
							// 	showCancel:false
							// })
						},
					})
				}
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
			onChagne1(e) {		// 是否多次提醒
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
			onChagne2(e) {		// 是否提醒
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
			onConfirm1(res, type) {	// 选择提醒时间
				this.time1 = res.result;
			},
			chooseCycle1(msg) {		// 选择多次提醒周期
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
