<template>
	<view>
		<form @submit="formSubmit()">
			<view class="shenfen">
				<view>身份</view>
			  <button class="button" @tap="handleTap('picker1')">{{ value1 }}</button>
			  <!-- <view>{{ value1 }}</view> -->
			  <lb-picker ref="picker1"
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
				<input class="input-box"></input>
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
					console.log("提交")
					wx.switchTab({
						url: '../views/partner'
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
					console.log(rsp);
					this.$set(this.urls, rsp.index, rsp.path);
					return;
					uni.uploadFile({
						url: '', //仅为示例，非真实的接口地址
						filePath: rsp.path,
						name: 'avatar',
						formData: {
							'avatar': rsp.path
						},
						success: (uploadFileRes) => {
							console.log(uploadFileRes.data);   //临时路径，怎么上传加书签了，到时候再看
						},
						complete(res) {
							console.log(res)
						}
					});
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
	justify-content: center;
	align-items:center;
}
.button{
/* 	margin-left: 20%; */
	margin-right: 10%;
	width: 70%;
	height: 80rpx;
	/* border: 1px solid #d8d8d8; */
}
.input-box{
	/* margin-right: 10%; */
	width: 70%;
	background-color: #f8f8f8;
	margin-left: 10%;
	height: 80rpx;
	border-radius: 5px;
	border: 1px solid #dedede;
	text-align: center;
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
</style>
