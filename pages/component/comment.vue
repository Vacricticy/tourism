<template>
	<view>
		<view @click="chooseImg">选择图片</view>
		<view class="imageList">
			<view v-for="(item,index) in imgSrc" :key="index">
				<image :src="item" mode="aspectFill"></image>
			</view>
		</view>

	</view>
</template>

<script>
	export default {
		data() {
			return {
				imgSrc: []
			}
		},
		methods: {
			chooseImg() {
				var that = this;
				uni.chooseImage({
					count: 9, //默认9
					sizeType: ['original', 'compressed'], //可以指定是原图还是压缩图，默认二者都有
					sourceType: ['album'], //从相册选择
					success: function(res) {
						// console.log(JSON.stringify(res.tempFilePaths));
						// that.imgSrc.push(JSON.stringify(res.tempFilePaths))
						// uni.previewImage({
						// 	urls: res.tempFilePaths
						// });
						if (that.imgSrc.length != 0) {
							that.imgSrc = that.imgSrc.concat(res.tempFilePaths)
						} else {
							that.imgSrc = res.tempFilePaths
						}
						console.log(that.imgSrc)
					}
				});
			}
		}
	}
</script>

<style>
	.imageList {
		display: flex;
		flex-wrap: wrap;
		justify-content: space-around;
	}

	image {
		width: 120px;
		height: 120px;
	}
</style>
