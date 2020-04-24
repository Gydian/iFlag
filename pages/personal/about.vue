<template>
	<view>
		<view class="logo-view">
			<image class="logo" src="../../static/logo.png"></image>
		</view>
		<view class="version-view">V1.0</view>
		<view class="about-view">
			<text decode="true">
				{{text.data[0].text}}
			</text>
		</view>
		<view class="author-view">copyright@iFlag-wiki粉丝后援会</view>
	</view>
</template>

<script>
	import about from "../../static/json/about.js"
	export default {
		data() {
			return {
				text: about,
			}
		},
	}
</script>

<style>
	.logo {
		width: 100px;
		height: 100px;
	}

	.logo-view {
		display: flex;
		justify-content: center;
		margin: 5%;
	}

	.version-view {
		display: flex;
		justify-content: center;
		margin: 5%;
		border-bottom: 1px solid lightgray;
	}

	.about-view {
		padding: 14px;
	}
	.author-view{
		font-size: 12px;
		display: flex;
		justify-content: center;
		margin-top: 60%;
	}
</style>
