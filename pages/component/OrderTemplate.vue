<template>
	<view class="sight" @click="showOrderDetail">
		<view class="picture">
			<image :src="order.viewPics.split(',')[0]" mode="aspectFill"></image>
		</view>
		<view class="content">
			<text class="sightName">{{order.viewName}}</text>
			<view class="price">
				下单时间：{{order.orderTime}}
			</view>
		</view>
		<view class="status">
			<image :src="statusList[order.status]" mode=""></image>
			<text v-if="order.status==0">待付款</text>
			<text v-if="order.status==1">待出行</text>
			<text v-if="order.status==2">已评价</text>
		</view>
	</view>
</template>

<script>
	import uniRate from '@/components/uni-rate/uni-rate.vue'
	export default {
		props: [
			'order', 'status'
		],
		components: {
			uniRate
		},
		data() {
			return {
				statusList: {
					2: '../../static/order/done.png',
					1: '../../static/order/comment.png',
					0: '../../static/order/money.png'
				}
			}
		},
		methods: {
			showOrderDetail() {
				uni.setStorageSync("status", this.order.status);
				uni.navigateTo({
					url: '/pages/component/OrderDetails?sightId=' + this.order.viewId + '&price=' + this.order.orderMoney / this.order
						.orderNum + '&pictureSrc=' + this.order.viewPics.split(',')[0] + '&isFromMyOrderPage=' + 'true' +
						'&totalPrice=' + this.order.orderMoney + '&num=' + this.order.orderNum + '&date=' + this.order.orderTime
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
				margin-top: 14px;
				font-size: 14px;
				color: red;
			}

		}
	}

	.status {
		width: 50px;
		height: 50px;

		image {
			width: 100%;
			height: 100%;
		}

		text {
			font-size: 14px;

		}
	}
</style>
