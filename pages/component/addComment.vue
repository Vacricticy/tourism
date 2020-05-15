<template>
	<view class="container">
		<image src="../../static/sights/sight2.jpg" mode="aspectFill"></image>
		<view class="box">
			<view class="score">
				<text>选择评分：</text>
				<uni-rate value="3.9" :disabled="false" size="25"></uni-rate>
				<text class="publish" @click="publish">发表</text>
			</view>
			<view class="content">
				<textarea value="" placeholder="从多个角度评价该景点,可以帮助更多想要参观该景点的人" />
				</view>
		</view>
	</view>
</template> 

<script>
	import uniRate from '@/components/uni-rate/uni-rate.vue'
	export default {
		components: {
			uniRate
		},
		data() {
			return {

			}
		},
		methods: {
			publish(){
				uni.showToast({
				    title: '发表成功',
				    duration: 1200
				});
				uni.switchTab({
				    url: '/pages/orders/Orders'
				})
			}
		}
	}
</script>

<style lang="less" scoped>
	.container {
		padding: 10px;
		background-color: #F1F1F1;
		// height: calc(100vh);
		display: flex;
		flex-direction: column;
		image{
			// height: calc(30vh);
			height: 320px;
			width: 100%;
		}
		.box{
			flex:1;
			margin-top: 10px;
			// height: 290px;
			// height: calc(60vh);
			width: 100%;
			background-color: #fff;
			border-radius: 5px;
			box-sizing: border-box;
			padding: 10px;
			padding-bottom: 20px;
			// display: flex;
			// flex-direction: column;
		}
	}
	.score{
		
		margin-top: 10px;
		margin-bottom: 19px;
		display: flex;
		font-size: 18px;
		.uni-rate{
			flex: 1;
			margin-top: 14px;
		}
		.publish{
			flex-basis: 10%;
		}
	}
	.content{
		display: flex;
	}
	textarea {
		// height: 100px;
		border: 1px solid #F1F1F1;
		border-radius: 5px;
		box-shadow: 0 0 10px #F1F1F1;
		padding: 5px;
		width: 100%;
		height: 290px; 
	}
	.publishBox{
		margin-top: 14px;
		display: flex;
		justify-content: flex-end;
	}
	.publish{
		margin-right: 6px;
		background-color: #ffb866;
		border-radius: 5px;
		box-shadow: 2px 2px 10px #d1d1d1;
		padding: 2px 5px 2px 5px;
	}
</style>
