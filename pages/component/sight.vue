<template>
	<view class="sight" @click="showSightDetail">
		<view class="picture">
			<image :src="sight.viewPics.split(',')[0]" mode="aspectFill"></image>
		</view>
		<view class="content">
			<text class="sightName">{{sight.viewName}}</text>
			<view class="evaluate">
				<uni-rate :value="sight.viewScore" disabled size="12"></uni-rate>
				<text class="score">{{sight.viewScore}}分</text>
				<text>| {{sight.viewMarkNum}}条评价</text>
			</view>
			<view class="distance">
				距我{{sight.distance}}km
			</view>
			<view class="price">
				￥{{sight.viewMoney}}
			</view>
		</view>
	</view>
</template>

<script>
	import uniRate from '@/components/uni-rate/uni-rate.vue'
	export default {
		props: [
			'sight'
		],
		components: {
			uniRate
		},
		data() {
			return {
				// sight: {}
			}
		},
		methods: {
			showSightDetail() {
				let url = '/pages/sightDetails/SightDetails?sightId=' + this.sight.id;
				uni.navigateTo({
					url: url
				})
			}
		}
	}
</script>

<style lang="less" scoped>
	.sight {
		// height: 120px;
		background-color: #fff;
		display: flex;
		margin-bottom: 10px;
		border-radius: 8px;
		box-sizing: border-box;
		padding: 10px;

		.picture {
			margin-right: 15px;
			width: 80px;
			height: 80px;
			border-radius: 5px;

			image {
				width: 100%;
				height: 100%;
				border-radius: 5px;
			}
		}

		.content {
			flex: 1;

			.sightName {
				font-weight: bold;
			}

			.evaluate {
				font-size: 10px;
				height: 20px;
				display: flex;
				align-items: center;

				.uni-rate {
					margin-top: 20px;
					height: 100%;
					margin-right: 8px;
				}

				.score {
					color: red;
					margin-right: 10px;
				}
			}

			.distance {
				font-size: 8px;
			}

			.price {

				color: red;
			}

		}
	}
</style>
