<template>
	<view>
		<scroll-view scroll-y="true" class="scroll-Y" show-scrollbar="false" @scrolltolower="showMore">
			<sight v-for="(item,index) in recommendSights" :key="index" :sight="item"></sight>
			<uni-load-more status="noMore"></uni-load-more>
		</scroll-view>
	</view>
</template>

<script>
	import sight from '../component/sight.vue'
	import uniLoadMore from "@/components/uni-load-more/uni-load-more.vue"
	export default {
		props: ['recommendSights'],
		components: {
			sight,
			uniLoadMore
		},
		created() {
			// this.getRecommendSights()
		},
		data() {
			return {
				// recommendSights: []
			}
		},
		methods: {
			// 获取推荐景点
			getRecommendSights() {
				uni.request({
					url: 'http://117.78.2.192:8090/view/info',
					method: 'POST',
					data: {
						latitude: 0,
						longitude: 0,
						model: 'recommand',
						name: ''
					},
					success: (res) => {
						// console.log(res.data.data.length)
						if (res.data.status == 200) {
							this.recommendSights = res.data.data;
							console.log(res.data.data[0].viewPics.split(',')[0])
						}
					}
				})
			},
			// 下拉刷新
			showMore() {
				// console.log(222222)
				// for (let i = 0; i < 4; i++) {
				// 	this.recommendSights.push({
				// 		id: '4',
				// 		imgSrc: '../../static/sights/sight4.jpg'
				// 	})
				// 	console.log(this.recommendSights.length)
				// }

			}
		}
	}
</script>

<style>
	.scroll-Y {
		height: calc(90vh);
		box-sizing: border-box;
		padding: 8px;
		background-color: #F1F1F1;
	}
</style>
