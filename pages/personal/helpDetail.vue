<template>
	<view>
		<view class="header">
			<text decode="true">
			{{str.data[index].question}}
			</text>
		</view>
		<view class="answer">
			<text decode="true">
			{{str.data[index].answer}}
			</text>
		</view>
	</view>
</template>

<script>
	import question from "../../static/json/question.js"
	export default {
		data() {
			return {
				str: question,
				index: ''
			}
		},
		onLoad: function(option) {
			this.index = option.question
		}
	}
</script>

<style>
	.header{
		font-size: 24px;
		text-align: center;
		margin: 4%;
	}
	.answer{
		font-size: 18px;
		text-align: left;
		padding: 4%;
	}
</style>
