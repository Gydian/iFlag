<template>
	<view>
		<image class="avatar" :src="partnerHead"></image>
		<uni-fab
		    :pattern="pattern"
		    :content="content"
		    :horizontal="horizontal"
		    :vertical="vertical"
		    :direction="direction"
		    @trigger="trigger"
			@fabClick="fabClick"
		></uni-fab>
	</view>
</template>

<script>
	import uniFab from '@/components/uni-fab/uni-fab.vue';
	export default {
	    components: {
	        uniFab
	    },
		data(){
			return{
				partnerHead:"../../static/logo.png",
				title: '聊天',
				horizontal: 'right',
				vertical: 'bottom',
				direction: 'horizontal',
				content: [{
						iconPath: '/static/image/一次性任务.png',
						selectedIconPath: '/static/image/一次性任务.png',
						text: '一次性',
						active: false
					},
					{
						iconPath: '/static/image/周期性任务.png',
						selectedIconPath: '/static/image/周期性任务.png',
						text: '周期性',
						active: false
					},
				]
			}
		},
		methods:{
			onNavigationBarButtonTap(e) {
			    console.log("success");
				uni.navigateTo({
					url:'../main/calendar'
				});
			},
			fabClick(){
				console.log("success");
				
			},
			trigger(e) {
				console.log(e)
				// this.content[e.index].active = !e.item.active
				// uni.showModal({
				// 	title: '提示',
				// 	content: `您${this.content[e.index].active ? '选中了' : '取消了'}${e.item.text}`,
				// 	success: function(res) {
				// 		if (res.confirm) {
				// 			console.log('用户点击确定')
				// 		} else if (res.cancel) {
				// 			console.log('用户点击取消')
				// 		}
				// 	}
				// })
				this.content[e.index].active = true
				if(e.item.text == "一次性"){
					console.log("11");
					uni.navigateTo({
						url:'../main/addFlag'
					});
				}
				else if(e.item.text == "周期性"){
					console.log("22");
					uni.navigateTo({
						url:'../main/addPerFlag'
					});
				}
			},
		},
		
	}
</script>

<style>
	.avatar {
		height: 50px;
		width: 50px;
		border-radius: 50%;
		-webkit-border-radius: 50%;
		-moz-border-radius: 50%;
		margin-top: 10%;
		margin-left: 5%;
	}
</style>
