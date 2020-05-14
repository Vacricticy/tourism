<template>
	<view>
		<scroll-view scroll-y="true" class="scroll-Y">
			<sight v-for="(item,index) in nearbySights" :key="index" :sight="item"></sight>
		</scroll-view>

	</view>
</template>

<script>
	import sight from '../component/sight.vue'
	export default {
		components: {
			sight
		},
		created() {
			this.getNearbySights()
		},
		data() {
			return {
				nearbySights: []
			}
		},
		methods: {
			getNearbySights() {
				this.nearbySights = [{
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
					}
				]
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
