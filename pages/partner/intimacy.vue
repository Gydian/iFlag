<template>
	<view>
		
		        <view class="uni-padding-wrap uni-common-mt">
		            <view class="text-box" scroll-y="true">
		                <text class="title">一、什么是对象亲密度？</text>
						<text class="content">通过一个百分比来体现你与对象的互动程度。</text>
						<text class="title">二、亲密度的百分比是如何计算的？</text>
						<text class="content">包含打卡的时长以及打卡的行为</text>
						<view class="text-box2">
							<image class="image" src="../../static/yuandian.png"></image>
							<text class="content2">打卡的时长指，有打卡行为的时长积</text>
						</view>
						<text class="content2">累（天）</text>
						<view class="text-box2">
							<!-- <view class="myimage">
								<image class="image" src="../../static/yuandian.png"></image>
							</view> -->
							<image class="image" src="../../static/yuandian.png"></image>
							<text class="content2">打卡的行为指，我向对象立flag的行为</text>
						</view>
						<text class="content2">以及成功完成flag的行为</text>
						<text class="title">三、怎样提高我和对象的亲密度百分比？</text>
						<view class="text-box2">
							<image class="image" src="../../static/yuandian.png"></image>
							<text class="content2">坚持长时间使用向对象打卡；</text>
						</view>
						<view class="text-box2">
							<image class="image" src="../../static/yuandian.png"></image>
							<text class="content2">多向对象立flag；</text>
						</view>
						<view class="text-box2">
							<image class="image" src="../../static/yuandian.png"></image>
							<text class="content2">多完成立下的flag，否则亲密度是会减</text>
						</view>
						<text class="content2">少的哦</text>
		            </view>
		        </view>
		    
</view>
</template>

<script>
	export default {
	    data() {
	        return {
	            texts:""
	        }
	    },
	    methods: {
	    }
	}

</script>

<style>
.text-box{
	margin-top: 3%;
	margin-left: 5%;
	margin-right: 5%;
	display: flex;
	flex-direction: column;
}
.title{
	font-size: 18px;
	font-weight:bold;
	margin-top: 5%;
}
.content{
	margin-top: 3%;
/* 	text-indent: 2em; */
	line-height: 25px;
}
.content2{
	line-height: 25px;
	margin-left: 1%;
	margin-left: 1%;
}
.text-box2{
	margin-top: 3%;
	display: flex;
	flex-direction: row;
	align-items: center;
/* 	justify-content: center; */
}
.image{
   height: 15px;
   width: 15px;
    border-radius: 50%;
    -webkit-border-radius: 50%;
    -moz-border-radius: 50%;
    margin-top: 1%;
/* 	margin: 1%; */
}
.myimage{
    height: 15px;
    width: 15px;
	overflow: hidden;
	border: 2px solid red;
}
</style>
