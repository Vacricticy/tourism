<template>
	<view>
		<view class="container">
			<view class="head">
				<view class="sightName">
					{{sightDetail.viewName}}
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
				<text class="addressName">四川省广安市广安区协兴镇</text>
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
				aaa:4
			}
		},
		created(){
			// console.log(this.sightDetail)/
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
