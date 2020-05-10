<template>
	<view>
		<form @submit="formSubmit()">
			<view class="shenfen">
				<view>身份</view>
			  <button class="button" @tap="handleTap('picker1')">{{ value1 }}</button>
			  <lb-picker class="lbpicker1" ref="picker1"
			    v-model="value1"
			    mode="selector"
			    :list="list1"
			    @change="handleChange"
			    @confirm="handleConfirm"
			    @cancle="handleCancle">
			  </lb-picker>			
			</view>
			<view class="nicheng">
				<view class="name">昵称</view>
				<input class="input-box" v-model="name"></input>
			</view>
			<view class="image">
				<view class="name">头像</view>
				<image :src="urls[1]" @click="clk(1)" class="myimg2"></image> 
				<avatar @upload="doUpload" @avtinit="doBefore" quality="1" ref="avatar"></avatar>
			</view>
			<view class="uni-btn-v">
			    <button form-type="submit">确认</button>
			</view>
			</form>
	    </view>
</template>

<script>
	import LbPicker from '@/components/lb-picker'
	import avatar from "../../components/yq-avatar/yq-avatar.vue";
	export default {
	        data() {
	            return {
					token:'',
					photo:'',
					isChange:false,
					name:"",
					value1: '请选择对象身份',
					list1: [
						{
							label: '家人',
							value: '家人'
						},
						{
							label: '爱豆',
							value: '爱豆'
						},
						{
							label: '宠物',
							value: '宠物'
						}
					],
					urls: ["../../static/logo.png","../../static/logo.png"],
	            }
	        },
			components: {
				LbPicker,
				avatar
			},
			onLoad: function() {
				var that = this;
				uni.getStorage({
					key:'email',
					success:function(res){
						console.log('这是key中的内容：'+res.data.token)
						that.token = res.data.token;
						uni.request({
							url: 'http://iflag.icube.fun:8080/ObjectCenter/'+res.data.token,
							method: "GET",
							sslVerify:false,
							success: function(response) {
								console.log(response.data)
								if(response.data.StatusCode==0){
									that.value1 = response.data.Messenger.identity;
									that.name = response.data.Messenger.nickname;
									that.urls[1] = "http://59.110.64.233:8080/user/object/"+that.token;
								}					
							},
							fail: function(response) {
								console.log(response.data);
							}
						});
					}
				})
			},
	        methods: {
				handleTap (name) {
					this.$refs[name].show()
				},
				handleChange (item) {
					// console.log('change::', item)
				},
				handleConfirm (item) {
					// console.log('confirm::', item)
				},
				handleCancle (item) {
					// console.log('cancle::', item)
				},
				formSubmit(){
					var that = this;
					console.log(this.value1)
					console.log(this.name)
					if(this.isChange==true){
						this.updateAvatar();
					}
					else{
						uni.uploadFile({
							url: 'http://59.110.64.233:8080/ObjectCenter/modify/',
							filePath: this.photo,
							name: 'avatar',
							header:{
								'Content-Type': 'multipart/form-data'
							},
							formData: {
								'token': this.token,
								'identity': this.value1,
								'nickname':this.name
							},
							success: function(uploadFileRes){
								console.log(uploadFileRes.data.StatusCode); 
								console.log(JSON.parse(uploadFileRes.data));
								if(JSON.parse(uploadFileRes.data).StatusCode==0){
									wx.switchTab({
										url: '../views/partner',
									});
								}
								else{
									uni.showModal({
										content: uploadFileRes.errMsg,
										showCancel:false
									})
								}							
							}
						});
					}				
				},
				updateAvatar(){
					console.log(this.photo)
					uni.uploadFile({
						url: 'http://59.110.64.233:8080/ObjectCenter/modify/',
						filePath: this.photo,
						name: 'avatar',
						header:{
							'Content-Type': 'multipart/form-data'
						},
						formData: {
							'token': this.token,
							'avatar': this.photo,
							'identity': this.value1,
							'nickname':this.name
						},
						success: (uploadFileRes) => {
							console.log(uploadFileRes.data); 
							if(JSON.parse(uploadFileRes.data).StatusCode==0){
								wx.switchTab({
									url: '../views/partner',
								});
							}
							else{
								uni.showModal({
									content: uploadFileRes.errMsg,
									showCancel:false
								})
							}
						}
					});
				},
				doBefore() {
					console.log('doBefore');
				},
				clk(index) {
					this.$refs.avatar.fChooseImg(index,{
						selWidth: '350upx', selHeight: '350upx', 
						expWidth: '260upx', expHeight: '260upx',
						inner: index ? 'true' : 'false'
					});
				},
				doUpload(rsp) {
					this.isChange=true;
					console.log(rsp);
					this.$set(this.urls, rsp.index, rsp.path);
					this.photo=rsp.path
					return;
				}
	        }
	    }
</script>

<style>
  .uni-form-item .title {
        padding: 20rpx 0;
    }
.shenfen{
	margin-top: 10%;
	margin-left: 5%;
	display: flex;
	flex-direction: row; 
/* 	justify-content: center; */
	align-items:center;
}
.button{
/* 	margin-left: 20%; */
/* 	margin-right: 10%; */
	width: 70%;
	height: 85rpx;
	margin-left: 10%;
	background-color: #f8f8f8;
	border-radius: 5px;
	border: 1px;
	text-align: center;
	font-size: 18px;
	align-items: center;
	justify-content: center;
	/* border: 1px solid #d8d8d8; */
}
.input-box{
	/* margin-right: 10%; */
	width: 70%;
	font-size: 18px;
	background-color: #f8f8f8;
	margin-left: 10%;
	height: 85rpx;
	border-radius: 5px;
	border: 1px solid #dedede;
	text-align: center;
	align-items: center;
	justify-content: center;
}
.nicheng{
	margin-top: 10%;
	margin-left: 5%;
	display: flex;
	flex-direction: row; 
	align-items:center;
}
.uni-btn-v{
	margin-top: 15%;
/* 	display: flex;
	flex-direction: row; 
	align-items:center; */
	/* width: 100%; */
	margin-left: 10%;
	margin-right: 10%;
}
.image{
	/* margin-top: 5%; */
	margin-left: 5%;
	display: flex;
	flex-direction: row; 
	align-items:center;
}
	.myimg2 {
		width: 100upx;
		height: 100upx;
		border-radius: 100%;
		margin: 40px;
	}
.lbpicker1{
	/* width: 70%;
	background-color: #f8f8f8; */
	/* margin-left: 10%; */
/* 	height: 80rpx;
	border-radius: 5px;
	border: 1px solid #dedede;
	text-align: center; */
	}
</style>
