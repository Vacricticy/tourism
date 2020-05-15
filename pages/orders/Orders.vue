<template>
	<view>
		<scroll-view scroll-y="true" class="scroll-Y" show-scrollbar="false" @scrolltolower="showMore">
			<order-template v-for="(item,index) in orderList" :key="index" :order="item"></order-template>
			<!-- <uni-load-more status="loading"></uni-load-more> -->
		</scroll-view>
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
				orderList: [{
					imgSrc: "../../static/sights/sight1.jpg",
					status: 0
				}, {
					imgSrc: "../../static/sights/sight2.jpg",
					status: 1
				}, {
					imgSrc: "../../static/sights/sight3.jpg",
					status: 2
				}]
			}
		},
		methods: {
			// 下拉刷新
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
	.scroll-Y {
		height: calc(100vh);
		box-sizing: border-box;
		padding: 8px;
		background-color: #F1F1F1;
	}
</style>
