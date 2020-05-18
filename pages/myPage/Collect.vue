<template>
	<view>
		<scroll-view scroll-y="true" class="scroll-Y" show-scrollbar="false" @scrolltolower="showMore">
			<sight v-for="(item,index) in collectList" :key="index" :sight="item"></sight>
			<uni-load-more status="noMore"></uni-load-more>
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
		data() {
			return {
				collectList: []
			}
		},
		created() {
			this.getData()
		},
		methods: {
			getData() {
				uni.request({
					url: 'http://117.78.2.192:8090/keep/' + uni.getStorageSync('userId'),
					success: (res) => {
						if (res.data.status == 200) {
							this.collectList = res.data.data;
						}
					}
				})
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
