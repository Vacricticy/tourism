<template>
	<view>
		<view class="page-body">
			<view class="page-section page-section-gap">
				<map style="width: 100%; height: 300px;" :latitude="latitude" :longitude="longitude" :markers="covers">
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
				}]
			}
		},
		onLoad() {
			this.getLocation();
		},
		methods: {
			getLocation() {
				this.longitude = uni.getStorageSync("longitude");
				this.latitude = uni.getStorageSync("latitude");
				this.covers[0].longitude = this.longitude;
				this.covers[0].latitude = this.latitude;
			}
		}
	}
</script>

<style>

</style>
