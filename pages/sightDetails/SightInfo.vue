<template>
	<view>
		<view class="container">
			<view class="head">
				<view class="sightName">
					邓小平故居
				</view>
				<view class="score">
					<text class="point">4.7</text>分
					<uni-rate value="3.9" disabled size="12"></uni-rate>
					<text class="num">1124条评价</text>
				</view>
			</view>
			<view class="time">
				正常营业 | 09:00开园
			</view>
			<view class="address" @click="map">
				<text class="addressName">四川省广安市广安区协兴镇</text>
				<text class="map">></text>
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
			map() {
				uni.navigateTo({
					url: '/pages/component/map'
				});
			}
		}
	}
</script>

<style lang="less" scoped>
	.container {
		width: 100%;
		border-radius: 8px;
		padding: 6px;
		background-color: #fff;
	}

	.head {
		display: flex;
		padding-bottom: 8px;
		border-bottom: 1px solid #C8C7CC;

		.sightName {
			font-weight: bold;
			flex: 1;
			padding-left: 10px;
			font-size: 24px;
		}

		.score {
			flex-basis: 34%;
			font-size: 15px;
			border-left: 1px solid #C8C7CC;
			padding-left: 5px;

			.point {
				font-size: 20px;
				font-weight: bold;

			}

			.uni-rate {
				margin-top: 5px;
				margin-bottom: 5px;
			}

			.num {
				color: red;
				font-size: 10px;
			}
		}
	}

	.time {
		font-size: 12px;
		line-height: 30px;
		border-bottom: 1px solid #C8C7CC;
	}

	.address {
		font-size: 15px;
		line-height: 30px;
		display: flex;
		cursor: pointer;

		.addressName {
			flex: 1
		}

		.map {

			flex-basis: 5%;
		}
	}
</style>
