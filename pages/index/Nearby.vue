<template>
	<view>
		<scroll-view scroll-y="true" class="scroll-Y">
			<sight v-for="(item,index) in nearbySights" :key="index" :sight="item"></sight>
			<uni-load-more status="noMore"></uni-load-more>
		</scroll-view>

	</view>
</template>

<script>
	import sight from '../component/sight.vue'
	import uniLoadMore from "@/components/uni-load-more/uni-load-more.vue"
	export default {
		props: ['nearbySights'],
		components: {
			sight,
			uniLoadMore
		},
		created() {
			// this.getNearbySights()
		},
		data() {
			return {
				// nearbySights: []
			}
		},
		methods: {
			getNearbySights() {
				uni.request({
					url: 'http://117.78.2.192:8090/view/info',
					method: 'POST',
					data: {
						latitude: 0,
						longitude: 0,
						model: 'distance',
						name: ''
					},
					success: (res) => {
						// console.log(res.data.data.length)
						if (res.data.status == 200) {
							this.nearbySights = res.data.data;
							console.log(res.data.data[0].viewPics.split(',')[0])
						}
					}
				})
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
