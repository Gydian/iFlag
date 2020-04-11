<template>
	<view>
		<view>
			<text class="text">反馈内容</text>
		</view>
		<view class="textarea">
			<textarea v-model="content" />
			</view> 
		<view>
			<text class="text">上传图片</text>
		</view>
		<view class="upload">
			<sunui-upimg @change="getImageInfo1" :upload_auto="true" ref="upimg1" :upload_count="2"></sunui-upimg>
		</view> 
		<view class="btn" @click="confirm()">
			<button>确认</button>
		</view>
	</view>
</template>

<script>
	import sunUiUpimg from '@/components/sunui-upimg/sunui-upimg.vue';
	export default {
		data() {
			return {
				content:''
			}
		},
		components: {
			'sunui-upimg': sunUiUpimg
		},
		methods:{
			getImageInfo1(e) {
				console.log('图片返回1：', e)
			},
			confirm(){
				if(this.content==''||this.content==null){
					uni.showModal({
						content: '请先填写反馈内容！',
						showCancel:false
					})
				}else{
					uni.showModal({
						content: '提交成功！',
						showCancel:false
					})
					uni.navigateBack({
						delta: 1
					});
				}
			}
		}
	}
</script>

<style>
	.text{
		margin: 5%;
		font-size: 18px;
		float: left;
	}
	.textarea{
		border: 1px solid lightgray;
		margin-left: 3%;
		margin-right: 3%;
		width: 90%;
		padding: 2%;
		float: left;
	}
	.upload{
		float: left;
		margin-top: 10%;
		margin-left: -28%;
	}
	.btn {
		float: left;
		border: 1px solid lightgray;
		width: 100%;
		margin-top: 50%;
	}
	button::after {
		border: none;
	}
</style>
