<template>
	<view>
		<!-- <view v-for="(x, y) in picList" :key="y" @tap="ViewImage"> -->
		<view @tap="clickPic(y)" v-for="(x, y) in picList" :key="y">
			<image :src="x.picPath"></image>
		</view>
		<!-- <image src="_doc/uniapp_temp_1589279002473/compressed/1589279016128.jpg" mode=""></image> -->
		<view class="" @click="map">
			查看地图
		</view>
		<view @click="comment">评论</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				picList: [],
				longitude: '',
				latitude: ''
			}
		},
		onLoad() {
			this.picListInit();
			this.getLocation();
		},
		methods: {
			picListInit() {
				// 图片列表数据初始化
				this.picList = [{
						picPath: 'https://ss3.bdstatic.com/70cFv8Sh_Q1YnxGkpoWK1HF6hhy/it/u=1387561229,2277799936&fm=26&gp=0.jpg'
					},
					{
						picPath: 'https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1576129362543&di=8eff78721202517fe7d369f50ec1075d&imgtype=0&src=http%3A%2F%2F5b0988e595225.cdn.sohucs.com%2Fimages%2F20171018%2Fc70c7af2fd374f42af70a073dc5115d8.jpeg'
					},
					{
						picPath: 'https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1576129382355&di=ac9a99b185716c7a4119c7e7bf9a548e&imgtype=0&src=http%3A%2F%2F5b0988e595225.cdn.sohucs.com%2Fimages%2F20170920%2Fc246838e40bf45b39ab0e816ac07d351.jpeg'
					},
					{
						picPath: 'https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1576129396706&di=d2789e97f05722ea77de07249e006048&imgtype=0&src=http%3A%2F%2Fuploads.xuexila.com%2Fallimg%2F1703%2F1008-1F3301FU3.jpg'
					},
					{
						picPath: 'https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1576129416485&di=788223df9463b93ce2a75964dd1ad6ca&imgtype=0&src=http%3A%2F%2Fhbimg.b0.upaiyun.com%2F57579d7ed11c73d6858ff723157eaa4b76182dbb35c34-t7mn6b_fw658'
					},
					{
						picPath: 'https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1576129447419&di=dbe09302e4c4eb8cf67994ab78c99e55&imgtype=0&src=http%3A%2F%2Fs6.sinaimg.cn%2Fmw690%2F001DLQZJzy78udj3LE165%26690'
					}
				];
				uni.setStorageSync('imgPreviewPicList', this.picList);
			},
			clickPic(index) {
				uni.setStorageSync("currentImgIndex", index);
				uni.navigateTo({
					url: '/pages/component/imgPreview'
				});
			},
			getLocation() {
				var that = this;
				uni.getLocation({
					type: 'wgs84',
					geocode: true,
					success: function(res) {
						console.log('当前位置的经度：' + res.longitude);
						console.log('当前位置的纬度2：' + res.latitude);
						this.longitude = res.longitude;
						this.latitude = res.latitude;
						uni.setStorageSync("longitude", this.longitude);
						uni.setStorageSync('latitude', this.latitude)
					}
				})
			},
			map() {
				uni.navigateTo({
					url: '/pages/component/map'
				});
			},
			comment() {
				uni.navigateTo({
					// url: '/pages/comment/comment'
				});
			},
			// ViewImage(e) {
			// 	console.log(e)
			// 	uni.previewImage({
			// 		urls: this.picList,

			// 		// current: e.currentTarget.dataset.url
			// 	});
			// },

		}
	}
</script>

<style>

</style>
