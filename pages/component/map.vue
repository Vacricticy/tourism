<template>
	<view>
		<view class="page-body">
			<view class="page-section page-section-gap">
				<map style="width: 100%; height: 750px;" :latitude="latitude" :longitude="longitude" :markers="covers" :polyline="polyline">
				</map>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				id: 0, // 使用 marker点击事件 需要填写id
				title: 'map',
				latitude: '',
				longitude: '',
				covers: [{
					latitude: '',
					longitude: '',
					iconPath: '/static/map/mapLocation.png'
				}, {
					latitude: '',
					longitude: '',
					iconPath: '/static/map/mapLocation.png'
				}],
				polyline: [{
					points: [{
						latitude: 0,
						longitude: 0
					}, {
						latitude: 0,
						longitude: 0
					}],
					color: '#000000',
					// width: 10
				}]
			}
		},
		onLoad(option) {
			this.longitude = option.lo;
			this.latitude = option.la;
			this.covers[0].longitude = uni.getStorageSync("lo");
			this.covers[0].latitude = uni.getStorageSync("la");
			this.covers[1].longitude = option.lo;
			this.covers[1].latitude = option.la;
			this.polyline[0].points[0].latitude = uni.getStorageSync("la");
			this.polyline[0].points[0].longitude = uni.getStorageSync("lo");
			this.polyline[0].points[1].latitude = option.la;
			this.polyline[0].points[1].longitude = option.lo;
		},
		methods: {}
	}
</script>

<style>

</style>
