<template>
	<view>
		<view v-for="item in list" :key="item.id">
			<uni-card :is-shadow="item.shadow" :title="item.title" mode="title" :thumbnail="item.thumbnail" :extra="item.extra"
			 :note="item.note">
				<view>
					<view class="image-box">
						<image class="image" mode="aspectFill" :src="item.src" />
					</view>
					<view class="content-box">
						<text class="content-box-text">
							{{item.content}}
						</text>
					</view>
				</view>
				<template slot="footer">
					<view class="footer-box">
						<view @click="share">
							<text class="footer-box__item">分享</text>
							<image class="foot-icon" src="../../static/image/share.png"></image>
						</view>
					</view>
				</template>
			</uni-card>
		</view>
	</view>
</template>

<script>
	import uniCard from '@/components/uni-card/uni-card.vue'
	export default {
		data() {
			return {
				name: '昵称',
				src: '../../static/logo.png',
				list: [
				// 	{
				// 	id: 0,
				// 	title: '标题文字',
				// 	content: '这是一个完整配置的基础卡片示例。内容样式可自定义。',
				// 	shadow: true,
				// 	note: 'Tips',
				// 	extra: '额外信息',
				// 	thumbnail: 'https://img-cdn-qiniu.dcloud.net.cn/new-page/uni.png',
				// 	src: 'https://img-cdn-qiniu.dcloud.net.cn/uniapp/images/shuijiao.jpg'
				// }, {
				// 	id: 1,
				// 	title: '标题文字',
				// 	content: '这是一个完整配置的基础卡片示例。内容样式可自定义。',
				// 	shadow: true,
				// 	note: 'Tips',
				// 	extra: '额外信息',
				// 	thumbnail: 'https://img-cdn-qiniu.dcloud.net.cn/new-page/uni.png',
				// 	src: 'https://img-cdn-qiniu.dcloud.net.cn/uniapp/images/shuijiao.jpg'
				// }, {
				// 	id: 2,
				// 	title: '标题文字',
				// 	content: '这是一个完整配置的基础卡片示例。内容样式可自定义。',
				// 	shadow: true,
				// 	note: 'Tips',
				// 	extra: '额外信息',
				// 	thumbnail: 'https://img-cdn-qiniu.dcloud.net.cn/new-page/uni.png',
				// 	src: 'https://img-cdn-qiniu.dcloud.net.cn/uniapp/images/shuijiao.jpg'
				// }, {
				// 	id: 3,
				// 	title: '标题文字',
				// 	content: '这是一个完整配置的基础卡片示例。内容样式可自定义。',
				// 	shadow: true,
				// 	note: 'Tips',
				// 	extra: '额外信息',
				// 	thumbnail: 'https://img-cdn-qiniu.dcloud.net.cn/new-page/uni.png',
				// 	src: 'https://img-cdn-qiniu.dcloud.net.cn/uniapp/images/shuijiao.jpg'
				// }
				],
				test:''
			}
		},
		components: {
			uniCard
		},
		onLoad:function(){
			var that = this
			uni.getStorage({
				key: 'email',
				success: function(res) {
					uni.request({
						url: 'http://59.110.64.233:8080/user/achievements/' + res.data.mail,
						method: "GET",
						sslVerify: false,
						success: function(response) {
							console.log(response)
							that.list=response.data
							for(let i=0;i<that.list.length;i++){
								that.list[i].thumbnail="http://iflag.icube.fun:8080/"+response.data[i].thumbnail
							}
							console.log(that.list)
							if(that.list==''||that.list==null){
								uni.showModal({
									content: '您还没有达成成就哦！',
									showCancel: false
								})
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
			share() {
				const that = this;
				/* 获取屏幕信息 */
				let ws = this.$mp.page.$getAppWebview();
				let bitmap = new plus.nativeObj.Bitmap('test');
				// 将webview内容绘制到Bitmap对象中
				ws.draw(
					bitmap,
					function(e) {
						/* 获取base64 */
						that.test = bitmap.toBase64Data();
						/* 加载base64编码 */
						bitmap.loadBase64Data(
							bitmap.toBase64Data(), 
							function() {
								console.log('加载Base64图片数据成功');
								/* 保存图片 */
								bitmap.save(
									'_doc/share.jpg', {},
									async (i) => {
											console.log('保存图片成功：' + JSON.stringify(i));
											uni.saveImageToPhotosAlbum({
												filePath: i.target,
												success: function() {
													/* 清除 */
													bitmap.clear();
													uni.showModal({
														content:'已成功保存到本地相册，可以去分享啦！',
														showCancel:false
													});
												},
												fail(e) {
													uni.showModal({
														content:'失败了！',
														showCancel:false
													});
												}
											});
										},
										function(e) {
											console.log('保存图片失败：' + JSON.stringify(e));
										}
								);
							},
							function() {
								console.log('加载Base64图片数据失败：' + JSON.stringify(e));
							}
						);
					},
					function(e) {
						console.log('截屏绘制图片失败：' + JSON.stringify(e));
					}, {
						check: true, // 设置为检测白屏
						clip: {
							top: '100px',
							left: '0px',
							height: '100%',
							width: '100%'
						} // 设置截屏区域
					}
				);
			}
		},

	}
</script>

<style>
	.image-box {
		/* #ifndef APP-NVUE */
		display: flex;
		flex-direction: column;
		/* #endif */
		height: 350rpx;
		overflow: hidden;
	}

	.image {
		/* #ifndef APP-NVUE */
		width: 100%;
		height: 100%;
		/* #endif */
		flex: 1;
	}

	.content-box {
		padding-top: 20rpx;
	}

	.content-box-text {
		font-size: 30rpx;
	}

	.footer-box {
		/* #ifndef APP-NVUE */
		display: flex;
		/* #endif */
		justify-content: space-between;
		flex-direction: row;

	}

	.footer-box__item {
		align-items: center;
		padding: 10rpx 0;
		font-size: 30rpx;
		color: #666;
	}

	.foot-icon {
		width: 15px;
		height: 15px;
		margin-left: 4px;
	}
</style>
