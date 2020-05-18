<template>
	<view class="container">
		<view class="">
			<scroll-view scroll-y="true" class="scroll-Y" show-scrollbar="false" @scrolltolower="showMore">
				<order-template v-for="(item,index) in orderList" :key="index" :order="item"></order-template>
				<uni-load-more status="noMore"></uni-load-more>
			</scroll-view>
		</view>
			
	</view>
</template>
<script>
	import orderTemplate from '../component/OrderTemplate.vue'
	import uniLoadMore from "@/components/uni-load-more/uni-load-more.vue"
	export default {
		components: {
			orderTemplate,
			uniLoadMore
		},
		data() {
			return {
				orderList: []
			}
		},
		created() {
			this.getAllOrders()
		},
		// 下拉刷新
		onLoad: function(options) {
			setTimeout(function() {
				console.log('start pulldown');
			}, 1000);
			uni.startPullDownRefresh();
		},
		onPullDownRefresh() {
			console.log('refresh');
			this.getAllOrders()
			setTimeout(function() {
				uni.stopPullDownRefresh();
			}, 1000);
		},
		methods: {
			//获取所有订单
			getAllOrders() {
				uni.request({
					url: 'http://117.78.2.192:8090/order/getAllOrders',
					method: 'GET',
					data: {
						userId: uni.getStorageSync("userId")
					},
					success: (res) => {
						if (res.data.status == 200) {
							this.orderList = res.data.data
							console.log(this.orderList)
						}
					}
				})
			},
			// 上拉刷新
			showMore() {
				console.log(222222)
				for (let i = 0; i < 4; i++) {
					this.orderList.push({
						imgSrc: '../../static/sights/sight4.jpg',
						statsu: 2
					})
				}

			}
		}
	}
</script>

<style lang="less" scoped>
	.container{
		height: calc(100%);
		background-color: #F1F1F1;
	}
	// .container{
	// 	background-color: #F1F1F1;
	// }
	.scroll-Y {
		height: calc(100%);
		box-sizing: border-box;
		padding: 8px;
		background-color: #F1F1F1;
	}
</style>
