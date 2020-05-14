<template>
	<view>
		<scroll-view scroll-y="true" class="scroll-Y" show-scrollbar="false" @scrolltolower="showMore">
			<sight v-for="(item,index) in recommendSights" :key="index" :sight="item"></sight>
			<uni-load-more status="loading"></uni-load-more>
		</scroll-view>
	</view>
</template>

<script>
	import sight from '../component/sight.vue'
	import uniLoadMore from "@/components/uni-load-more/uni-load-more.vue"
	export default {
		components: {
			sight,
			uniLoadMore
		},
		created() {
			this.getRecommendSights()
		},
		data() {
			return {
				recommendSights: []
			}
		},
		methods: {
			// 获取推荐景点
			getRecommendSights() {
				this.recommendSights = [{
						id: '1',
						imgSrc: '../../static/sights/sight1.jpg'
					},
					{
						id: '2',
						imgSrc: '../../static/sights/sight2.jpg'
					},
					{
						id: '3',
						imgSrc: '../../static/sights/sight3.jpg'
					},
					{
						id: '4',
						imgSrc: '../../static/sights/sight4.jpg'
					},
					{
						id: '3',
						imgSrc: '../../static/sights/sight3.jpg'
					},
					{
						id: '4',
						imgSrc: '../../static/sights/sight4.jpg'
					}
				]
			},
			// 下拉刷新
			showMore() {
				console.log(222222)
				for (let i = 0; i < 4; i++) {
					this.recommendSights.push({
						id: '4',
						imgSrc: '../../static/sights/sight4.jpg'
					})
					console.log(this.recommendSights.length)
				}

			}
		}
	}
</script>

<style>
	.scroll-Y {
		height: calc(100vh);
		box-sizing: border-box;
		padding: 8px;
		background-color: #F1F1F1;
	}
</style>
