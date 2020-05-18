<template>
	<view class="content">
		<view class="navigation">
			<view :class="[current==0?'sel':'','nav']" @click="current=0">推荐</view>
			<view :class="[current==1?'sel':'','nav']" @click="current=1">附近</view>
		</view>
		<view class="loction_search">
			<view class="location" @click="getMap">
				<text>{{location}}</text>
				<image src="../../static/index/location.png" mode="widthFix"></image>
			</view>
			<view class="search">
				<input confirm-type="search" @confirm="search" value="" placeholder="搜索景点" v-model="searchName" />
			</view>
		</view>

		<swiper :current="current" @change="changeNav" class="swiper">
			<swiper-item>
				<recommend :recommendSights="recommendSights"></recommend>
			</swiper-item>
			<swiper-item>
				<nearby :nearbySights="nearbySights"></nearby>
			</swiper-item>
		</swiper>
	</view>
</template>

<script>
	import recommend from './Recommend.vue'
	import nearby from './Nearby.vue'

	export default {
		components: {
			recommend,
			nearby,

		},
		created() {
			this.getRecommendData()
		},
		data() {
			return {
				current: 0,
				location: '',
				recommendSights: [],
				nearbySights: [],
				searchName: '',
				latitude: 0,
				longitude: 0,
				lalo: {}
			}
		},
		methods: {
			async getRecommendData() {
				let lalo = await this.getLocation();
				this.lalo = lalo;
				this.getRecommendSights(lalo)
			},
			async getNearbyData() {
				let lalo = await this.getLocation();
				this.lalo = lalo;
				this.getNearbySights(lalo)
			},
			changeNav(e) {
				this.current = e.detail.current
				if (this.current == 0) {
					this.getRecommendData()
				} else if (this.current == 1) {
					this.getNearbyData()
				}
			},
			// 获取推荐页的景点详情
			getRecommendSights(lalo) {
				// console.log(this.longitude)
				uni.request({
					url: 'http://117.78.2.192:8090/view/info',
					method: 'POST',
					data: {
						latitude: lalo.latitude,
						longitude: lalo.longitude,
						model: 'recommand',
						name: ''
					},
					success: (res) => {
						if (res.data.status == 200) {
							this.recommendSights = res.data.data;
						}
					}
				})
			},
			// 获取附近页的景点详情
			getNearbySights(lalo) {
				uni.request({
					url: 'http://117.78.2.192:8090/view/info',
					method: 'POST',
					data: {
						latitude: lalo.latitude,
						longitude: lalo.longitude,
						model: 'distance',
						name: ''
					},
					success: (res) => {
						if (res.data.status == 200) {
							this.nearbySights = res.data.data;
						}
					}
				})
			},
			// 获取位置信息
			getLocation() {
				return new Promise((resolve, reject) => {
					var that = this;
					uni.getLocation({
						type: 'wgs84',
						geocode: true,
						success: function(res) {
							this.longitude = res.longitude;
							this.latitude = res.latitude;
							console.log('当前位置的经度：' + this.longitude);
							console.log('当前位置的纬度2：' + this.latitude);
							console.log(res.address.city)
							that.location = res.address.city;
							let lalo = {
								longitude: res.longitude,
								latitude: res.latitude
							}
							//保存经纬度，便于访问地图时连线操作
							uni.setStorageSync("la", res.latitude);
							uni.setStorageSync("lo", res.longitude);
							resolve(lalo);
						}
					})
				})
			},
			// 跳转至地图界面
			getMap() {
				uni.navigateTo({
					url: '/pages/component/map?la=' + this.lalo.latitude + '&lo=' + this.lalo.longitude
				});
			},
			// 搜索功能
			search() {
				uni.navigateTo({
					url: '/pages/component/searchResult?searchName=' + this.searchName + '&la=' + this.lalo.latitude + '&lo=' + this
						.lalo.longitude
				})
			}
		}
	}
</script>

<style lang="less" scoped>
	.content {
		height: calc(100vh);
		display: flex;
		flex-direction: column;
	}

	.navigation {
		display: flex;
		justify-content: center; //水平居中
		border-bottom: 1rpx solid #C8C7CC;

		.sel {
			color: #02c8ff;
		}

		.nav {
			margin-left: 10rpx;
			margin-right: 10rpx;
		}
	}

	.swiper {
		flex: 1;
	}

	.loction_search {
		display: flex;
		padding: 5px;
		background-color: #F1F1F1;

		.location {
			color: #fff;
			text-align: center;
			flex: 1;
			font-size: 15px;
			height: 20px;
			background-color: #02c8ff;
			border-radius: 8px;
			margin-right: 10px;
			padding-left: 7px;
			padding-right: 5px;
			display: flex;
			cursor: pointer;

			text {
				flex: 1
			}

			image {
				width: 20px;
				height: 20px;
			}
		}

		.search {
			height: 100%;
			box-sizing: border-box;
			padding-left: 5px;
			flex-basis: 70%;
			border: 1px solid #C8C7CC;
			border-radius: 8px;
			box-shadow: 0 0 10px #C8C7CC;
		}

		.searchButtom {
			margin-left: 10px;
			font-size: 15px;
			background-color: #fff;
			border-radius: 4px;
			box-shadow: 2px 2px 6px #999999;
			padding-left: 2px;
			padding-right: 2px;
		}
	}
</style>
