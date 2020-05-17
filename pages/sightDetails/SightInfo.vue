<template>
	<view>
		<view class="container">
			<view class="head">
				<view class="sightName">
					<text>{{sightDetail.viewName}}</text>
					<text class="baidu" @click="baidu">百度百科</text>
				</view>
				<view class="score">
					<text class="point">{{sightDetail.viewScore}}</text>分
					<uni-rate :value="aaa" disabled size="12"></uni-rate>
					<text class="num">{{sightDetail.viewMarkNum}}条评价</text>
				</view>
			</view>
			<view class="time">
				正常营业 | {{sightDetail.viewStartTime}}开园,{{sightDetail.viewStopTime}}闭馆
			</view>
			<view class="address" @click="map">
				<text class="addressName">点击查看位置</text>
				<text class="map">></text>
			</view>
		</view>
	</view>

</template>

<script>
	import uniRate from '@/components/uni-rate/uni-rate.vue'
	export default {
		props: [
			'sightDetail'
		],
		components: {
			uniRate
		},
		data() {
			return {
				aaa: 4
			}
		},
		created() {
			// console.log(this.sightDetail)/
		},
		methods: {
			map() {
				console.log(this.sightDetail)
				uni.navigateTo({
					url: '/pages/component/map?la=' + this.sightDetail.viewLatitude + '&lo=' + this.sightDetail.viewLongitude
				});
			},
			baidu() {
				uni.navigateTo({
					url: '/pages/component/baidu?param=' + this.sightDetail.viewName
				})
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
			display: flex;
			flex-direction: column;

			.baidu {
				margin-left: -10px;
				margin-top: 5px;
				text-align: center;
				width: 30%;
				font-size: 14px;
				background-color: #C0C0C0;
				color: #fff;
				border-radius: 5px;
				box-shadow: 2px 2px 5px #999999;
			}
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
