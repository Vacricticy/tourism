<template>
	<view class="container">
		<image :src="pictureSrc" mode="aspectFill"></image>
		<view class="order">
			<view class="date viewBox">
				<text @click="open" class="textBox" v-if="!isFromMyOrderPage">选择日期</text>
				<text class="textBox" v-else>预定日期</text>
				<text>{{date}}</text>
			</view>

			<view class="num viewBox">
				<text class="textBox">购买数量</text>
				<uni-number-box :value="orderNum" :min="0" @change="bindChange" v-if="!isFromMyOrderPage"></uni-number-box>
				<text v-else>{{orderNum}}</text>
			</view>
			<view class="oneTicketMoney viewBox">
				<text class="textBox">单价</text>
				<text>￥{{price}}</text>
			</view>
			<view class="money viewBox">
				<text class="textBox">订单金额</text>
				<text v-if="!isFromMyOrderPage">￥{{allMoney}}</text>
				<text v-else>￥{{totalPrice}}</text>
			</view>
			<view class="status">
				<view class="imageBox">
					<image :src="statusList[status]" mode=""></image>
					<text v-if="status==0">待付款</text>
					<text v-if="status==1">待出行</text>
					<text v-if="status==2">已评价</text>
				</view>
				<view class="handle" v-if="status==-1">
					<text class="textBox" @click="preOrder">立即预定</text>
				</view>
				<view class="handle" v-if="status==0">
					<text class="textBox orderUnpayed" @click="pay">立即付款</text>
				</view>
				<view class="handle" v-if="status==1">
					<text class="textBox" @click="addComment">立即评价</text>
				</view>
				<view class="handle" v-if="status==2">
					<text class="textBox orderDone">已评价</text>
				</view>
			</view>
			<view>
				<uni-calendar ref="calendar" :insert="false" @confirm="confirm" />
			</view>
		</view>

	</view>
</template>

<script>
	import uniCalendar from '@/components/uni-calendar/uni-calendar.vue'
	import uniNumberBox from "@/components/uni-number-box/uni-number-box.vue"
	export default {
		components: {
			uniCalendar,
			uniNumberBox
		},
		data() {
			return {
				date: '',
				num: 0,
				price: 50,
				statusList: {
					2: '../../static/order/done.png',
					1: '../../static/order/comment.png',
					0: '../../static/order/money.png'
				},
				status: 0,
				sightId: 0,
				orderNum: 1,
				totalPrice: 0,
				isFromMyOrderPage: false,
				orderStatus: 0,
				orderId: 0 //来源1：预定之后直接产生，来源2：从我的订单页面传递参数
			}
		},
		onLoad(option) {
			this.sightId = option.sightId
			this.price = option.price
			this.pictureSrc = option.pictureSrc
			if (option.isFromMyOrderPage) {
				this.isFromMyOrderPage = true
			}
			let today = new Date()
			let todayDate = today.getFullYear() + '-' + (today.getMonth() + 1) + '-' + today.getDate()
			this.orderNum = option.num ? option.num : 1
			this.totalPrice = option.totalPrice ? option.totalPrice : 0
			this.date = option.date ? option.date : todayDate
			this.status = option.orderStatus ? option.orderStatus : 0
			this.orderId = option.orderId ? option.orderId : 0
			// console.log(this.sightId)
		},
		created() {
			// let today = new Date()
			// this.date = today.getFullYear() + '-' + (today.getMonth() + 1) + '-' + today.getDate()
		},
		methods: {
			open() {
				this.$refs.calendar.open();
			},
			confirm(e) {
				this.date = e.fulldate;
			},
			bindChange(e) {
				this.num = e
			},
			addComment() {
				uni.navigateTo({
					url: '/pages/component/addComment?orderId=' + this.orderId + '&sightId=' + this.sightId
				})
			},
			// 预定功能
			preOrder() {
				// console.log(this.num * this.price)
				// console.log(this.num)
				// console.log(uni.getStorageSync("userId"))
				uni.request({
					url: 'http://117.78.2.192:8090/order/reserve',
					method: 'POST',
					data: {
						"orderMoney": this.num * this.price,
						"orderNum": this.num,
						"userId": uni.getStorageSync("userId"),
						"viewId": this.sightId
					},
					success: (res) => {
						if (res.data.status == 200) {
							console.log('ggggggg' + res.data.data)
							this.orderId = res.data.data
							uni.showToast({
								title: "预定成功",
								duration: 1500
							})
							this.status = 0
						}
					}
				})
			},
			//支付功能
			pay() {
				uni.request({
					url: 'http://117.78.2.192:8090/order/pay',
					method: 'GET',
					data: {
						dataId: this.orderId
					},
					success: (res) => {
						if (res.data.status == 200) {
							uni.showToast({
								title: "支付成功",
								duration: 1500
							})
							this.status = 1
						}
					}
				})
			},
		},
		computed: {
			allMoney() {
				return this.num * this.price
			}
		}
	}
</script>

<style lang="less" scoped>
	image {
		width: 100%;
		height: 450rpx;
	}

	.container {
		background-color: #F1F1F1;
		padding: 8px;
	}

	.order {
		background-color: #fff;
		border-radius: 5px;
		display: flex;
		flex-direction: column;
	}

	.textBox {
		background-color: #39baff;
		color: #fff;
		// margin: 3px;
		border-radius: 5px;
		padding: 5px;
		box-shadow: 2px 2px 5px #C8C7CC;
	}

	.viewBox {
		margin: 10px;
		display: flex;
		justify-content: space-between;
	}

	.date {
		margin-top: 10px;

		.textBox {
			// border: 1px solid #C8C7CC;
			box-shadow: 2px 2px 5px #C8C7CC;
		}
	}

	.oneTicketMoney {
		.textBox {
			background-color: #F0AD4E;
		}
	}

	.num {
		.textBox {
			background-color: #7bcc3d;
		}
	}

	.money {
		color: red;

		.textBox {
			background-color: #f06c4b;
		}
	}

	.status {
		border-top: 1px solid #F1F1F1;
		height: 165px;
		margin: 10px;
		// font-size: 14px;
		text-align: center;
		display: flex;
		justify-content: space-between;
		align-items: center;

		.imageBox {
			margin-left: 10px;
			width: 80px;
			height: 80px;

			image {
				width: 100%;
				height: 100%;
			}
		}

		.handle {}
	}

	.postComment {
		height: 200px;
		background-color: #007AFF;
	}

	.orderDone {
		background-color: #49d360;
	}

	.orderUnpayed {
		background-color: #f06c4b;
	}
</style>
