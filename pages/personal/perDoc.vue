<template>
	<view>
		<view>
			<uni-nav-bar left-icon="back" left-text="返回" right-text="保存" title="个人信息" @clickLeft="back()"></uni-nav-bar>
		</view>
		<view class="avatar-view">
			<view class="text">头像</view>
			<view>
				<image class="enter-icon" src="../../static/image/向右.png"></image>
			</view>
			<view>
				<image class="avatar" src="../../static/logo.png"></image>
			</view>
		</view>
		<view class="avatar-view">
			<view class="text">昵称</view>
			<input class="name-input" v-model="info.name"/>
		</view>
		<view class="avatar-view">
			<view class="text">性别</view>
			<button class="sex-btn" @click="choosePop()">{{info.sex}}</button>
		</view>
		<uni-popup ref="popup" type="bottom">
			<view class="pop-view">
				<view class="pop-text">性别</view>
				<button class="pop-btn" @click="chooseSex('男')">男</button>
				<button class="pop-btn" @click="chooseSex('女')">女</button>
				<button class="pop-btn" @click="chooseSex('未知')">未知</button>
			</view>
		</uni-popup>
	</view>
</template>

<script>
	import uniNavBar from "@/components/uni-nav-bar/uni-nav-bar.vue"
	import uniPopup from "@/components/uni-popup/uni-popup.vue"
	export default {
		data() {
			return {
				info:{
					name:'这是个昵称',
					avatar:'',
					sex:'女'
				}
			}
		},
		components: {
			uniNavBar,
			uniPopup
		},
		onLoad: function() {
			// 获取个人信息
		},
		methods: {
			back() {
				uni.navigateBack({
					delta: 1
				});
			},
			choosePop(){
				this.$refs.popup.open()
			},
			chooseSex(sex){
				this.info.sex=sex
			}
		}
	}
</script>

<style>
	.avatar {
		height: 50px;
		width: 50px;
		border-radius: 50%;
		-webkit-border-radius: 50%;
		-moz-border-radius: 50%;
		float: right;
		margin: 3%;
	}

	.enter-icon {
		height: 25px;
		width: 25px;
		float: right;
		margin-top: 6%;
	}

	.avatar-view {
		height: 70px;
		padding-left: 14px;
		padding-right: 14px;
		border-bottom: 1px solid lightgray;
		border-radius: 0;
		background-color: #fff;
	}
	
	.text{
		float: left;
		margin-top: 7%;
	}
	
	.name-input{
		float: right;
		margin-top: 7%;
		width: 200px;
		text-align: right;
	}
	
	.sex-btn{
		background-color: #fff;
		border: none;
		text-align: right;
		float: right;
		width: 200px;
		margin-top: 3%;
	}
	button::after {
		border: none;
	}
	
	.pop-view{
		height: 200px;
		background-color: #fff;
		text-align: center;
	}
	.pop-text{
		margin: 5%;
		padding-top: 5%;
		font-size: 18px;
	}
	.pop-btn{
		text-align: left;
		background-color: #fff;
		margin-left: 14px;
		margin-right: 14px;
		border-radius: 0;
		border-top: 1px solid lightgray;
	}
</style>
