<template>
	<view>
		<view class="text">常见问题</view>
		<navigator url="../personal/helpDetail?question=0" open-type="navigate">
			<button class="navigate-btn">
				怎么修改角色头像?
				<view>
					<image class="enter-icon" src="../../static/image/向右.png"></image>
				</view>
			</button>
		</navigator>
		<navigator url="../personal/helpDetail?question=1" open-type="navigate">
			<button class="navigate-btn">
				怎么新建Flag?
				<view>
					<image class="enter-icon" src="../../static/image/向右.png"></image>
				</view>
			</button>
		</navigator>
		<navigator url="../personal/helpDetail?question=2" open-type="navigate">
			<button class="navigate-btn">
				怎么设置提醒打卡?
				<view>
					<image class="enter-icon" src="../../static/image/向右.png"></image>
				</view>
			</button>
		</navigator>
		<navigator url="../personal/feedback" open-type="navigate">
			<view>
				<button class="feedback-btn">
					<image class="feedback-icon" src="../../static/image/反馈.png"></image>
					<text class="feedback-text">我要反馈</text>
				</button>
			</view>
		</navigator>
	</view>
</template>

<script>
	export default {
		data() {
			return {

			}
		}
	}
</script>

<style>
	.text {
		height: 50px;
		text-align: left;
		padding-top: 25px;
		padding-left: 14px;
		border-bottom: 1px solid lightgray;
	}

	.navigate-btn {
		border-bottom: 1px solid lightgray;
		border-radius: 0;
		height: 50px;
		background-color: #fff;
		text-align: left;
	}

	button::after {
		border: none;
	}

	.enter-icon {
		height: 22px;
		width: 22px;
		float: right;
		margin-right: 5%;
		margin-top: -34px;
	}

	.feedback-icon {
		height: 22px;
		width: 22px;
		margin-right: 3%;
		margin-top: 3%;
		margin-left: 35%;
		float: left;
	}

	.feedback-btn {
		border: 1px solid lightgray;
		margin-top: 80%;
	}
	.feedback-text{
		float: left;
	}
</style>
