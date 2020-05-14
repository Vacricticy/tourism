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
				<input type="text" value="" placeholder="搜索景点" />
			</view>
		</view>
		<swiper :current="current" @change="changeNav" class="swiper">
			<swiper-item>
				<recommend></recommend>
			</swiper-item>
			<swiper-item>
				<nearby></nearby>
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
			this.getLocation()
		},
		data() {
			return {
				current: 0,
				location: ''
			}
		},
		onLoad() {

		},
		methods: {

			changeNav(e) {
				this.current = e.detail.current
			},
			// 获取位置信息
			getLocation() {
				var that = this;
				uni.getLocation({
					type: 'wgs84',
					geocode: true,
					success: function(res) {
						console.log('当前位置的经度：' + res.longitude);
						console.log('当前位置的纬度2：' + res.latitude);
						console.log(res.address.city)
						that.location = res.address.city;
					}
				})
			},
			// 跳转至地图界面
			getMap(){
				uni.navigateTo({
					url: '/pages/component/map'
				});
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
			text{
				flex:1
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
			flex-basis: 80%;
			border: 1px solid #C8C7CC;
			border-radius: 8px;
			box-shadow: 0 0 10px #C8C7CC;
		}
	}
</style>
