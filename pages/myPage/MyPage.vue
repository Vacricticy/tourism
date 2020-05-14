<template>
	<view>
		<view class="container">
			<view class="bg_img">
				<view class="myInfo">
					<image :src="url" mode=""></image>
				</view>
				<view class="nickname">
					evenliu-evenliu
				</view>
			</view>

			<view class="myCollect">
				<view class="collectTitle">
					<text class="titleBox">我的收藏</text>
					<text class="more" @click="more">查看更多</text>
				</view>
				<swiper class="swiper" indicator-dots="indicatorDots" autoplay="autoplay" interval="interval" duration="duration">
					<swiper-item v-for="(item,index) in sightList" :key="index">
						<sight :sight="item"></sight>
					</swiper-item>
				</swiper>
			</view>

			<view class="info">
				<view class="infoTitle">
					<text class="titleBox">个人信息</text>
				</view>
				<view class="infoContent">
					<view class="id xxxTitle">
						<text class="xxxtitleBox">帐号</text>
						<text>123123</text>
					</view>
					<view class="nickname xxxTitle">
						<text class="xxxtitleBox">昵称</text>
						<text>evenliu-evenliu</text>
					</view>
					<view class="avatar xxxTitle">
						<!-- <text @click="" class="xxxtitleBox">上传头像</text> -->
						<avatar  selWidth="200px" selHeight="400upx" @upload="myUpload" :avatarSrc="url" avatarStyle="width: 200upx; height: 200upx; border-radius: 100%;">
						</avatar>
					</view>
				</view>

			</view>
		</view>

	</view>
</template>

<script>
	import sight from "../component/sight.vue"
	import avatar from "../../components/yq-avatar/yq-avatar.vue";
	export default {
		components: {
			sight,
			avatar
		},
		data() {
			return {
				sightList: [{
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
					}
				],
				url: "../../static/sights/sight1.jpg"
			}
		},
		methods: {
			more() {
				uni.navigateTo({
					url: '/pages/myPage/Collect'
				})
			},
			// uploadAvatar() {
			// 	uni.navigateTo({
			// 		url: '/pages/myPage/UploadAvatar'
			// 	})
			// },
			myUpload(rsp) {
				this.url = rsp.path; //更新头像方式一
				console.log(this.url)
				//rsp.avatar.imgSrc = rsp.path; //更新头像方式二
			}
		},
	}
</script>

<style lang="less" scoped>
	.container {
		display: flex;
		flex-direction: column;
	}

	.bg_img {
		height: 430rpx;
		background: url(../../static/sights/sight3.jpg);
		background-size: 100% 100%;
		display: flex;
		flex-direction: column;
		justify-content: center;
		align-items: center;
		color: #fff;
	}

	.nickname {}

	.myInfo {

		width: 70px;
		height: 70px;
		border-radius: 50%;
		box-shadow: 0 0 10px #b3b3b6;
		margin-right: 10px;
		border: 1px solid #F1F1F1;
		padding: 1px;
		background-color: #fff;
		box-shadow: 0 0 10px #F1F1F1;

		image {
			width: 100%;
			height: 100%;
			border-radius: 50%;
		}
	}

	.myCollect {
		flex: 1;
		background-color: #F1F1F1;
		padding: 4px;

		.collectTitle {
			margin-top: 2px;
			border-radius: 6px;
			padding: 5px;
			background-color: #fff;
			letter-spacing: 0.06em;
			margin-bottom: 10px;
			display: flex;
			justify-content: space-between;

			.titleBox {
				font-size: 20px;
				padding: 2px;
				border-radius: 5px;
				color: #fff;
				background-color: #ffb573;
				margin-right: 10px;
				font-weight: bold;
			}

			.more {
				font-size: 18px;
				padding: 2px;
				border-radius: 5px;
				color: #fff;
				background-color: #6ad8ff;
			}
		}
	}

	.info {
		flex: 1;
		background-color: #F1F1F1;
		padding: 4px;

		.infoTitle {
			margin-top: 2px;
			border-radius: 6px;
			padding: 5px;
			background-color: #fff;
			letter-spacing: 0.06em;
			margin-bottom: 10px;
			display: flex;
			justify-content: space-between;

			.titleBox {
				font-size: 20px;
				font-weight: bold;
				padding: 2px;
				border-radius: 5px;
				color: #fff;
				background-color: #ffb573;
				margin-right: 10px;
			}
		}

		.infoContent {
			.id {
				display: ;
			}
		}
	}

	.xxxTitle {
		margin-top: 2px;
		border-radius: 6px;
		padding: 5px;
		background-color: #fff;
		letter-spacing: 0.06em;
		margin-bottom: 10px;
		display: flex;
		justify-content: space-between;
	}
	.xxxtitleBox {
		line-height: 28px;
		font-size: 15px;
		padding: 2px;
		border-radius: 5px;
		color: #fff;
		background-color: #6ad8ff;
	}
</style>
