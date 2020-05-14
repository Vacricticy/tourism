<template>
	<view>
		<view class="container">
			<view class="myInfo">
				<image src="../../static/sights/sight1.jpg" mode=""></image>
			</view>
			<view class="myCollect">
				<view class="collectTitle">
					我的收藏
				</view>

			</view>
		</view>

	</view>
</template>

<script>
	export default {
		data() {
			return {

			}
		},
		methods: {

		}
	}
</script>

<style lang="less" scoped>
.myInfo{
	height:200px;
	background-color: #007AFF;
}
.myInfo {
			width: 40px;
			height: 40px;
			border-radius: 50%;
			box-shadow: 0 0 10px #b3b3b6;
			margin-right: 10px;

			image {
				width: 100%;
				height: 100%;
				border-radius: 50%;
			}
		}
</style>
