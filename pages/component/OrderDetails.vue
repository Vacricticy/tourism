<template>
	<view class="container">
		<image src="../../static/sights/sight2.jpg" mode="aspectFill"></image>
		<view class="order">
			<view class="date viewBox">
				<text @click="open" class="textBox">选择日期</text>
				<text>{{date}}</text>
			</view>

			<view class="num viewBox">
				<text class="textBox">购买数量</text>
				<uni-number-box :min="0" @change="bindChange"></uni-number-box>
			</view>
			<view class="oneTicketMoney viewBox">
				<text class="textBox">单价</text>
				<text>￥{{price}}</text>
			</view>
			<view class="money viewBox">
				<text class="textBox">订单金额</text>
				<text>￥{{allMoney}}</text>
			</view>
			<view class="status">
				<view class="imageBox">
					<image :src="statusList[status]" mode=""></image>
					<text v-if="status==0">待付款</text>
					<text v-if="status==1">待出行</text>
					<text v-if="status==2">已评价</text>
				</view>
				<view class="handle" v-if="status==0">
					<text class="textBox" @click="pay">立即付款</text>
				</view>
				<view class="handle" v-if="status==1">
					<text class="textBox" @click="addComment">立即评价</text>
				</view>
				<view class="handle" v-if="status==2">
					<text class="textBox">已评价</text>
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
				status: 0
			}
		},
		created() {
			this.status = uni.getStorageSync("status");
			console.log("*****" + uni.getStorageSync("status"))
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
				console.log(e)
			},
			addComment() {
				uni.navigateTo({
					url: '/pages/component/addComment'
				})
			},
			async pay() {
				await uni.showToast({
					title: "支付成功",
					duration: 1500
				})
				this.status=1
			}
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
</style>
