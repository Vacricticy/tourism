<template>
	<view class="container">
		<swiper class="swiper" indicator-dots="indicatorDots" autoplay="autoplay" interval="interval" duration="duration">
			<swiper-item v-for="(item,index) in sightSrc" :key="index" @tap="clickPic(index)">
				<image :src="item.imgSrc" mode="aspectFill"></image>
			</swiper-item>
		</swiper>
		<view class="content">
			<!-- 景点信息 -->
			<view class="infoBox">
				<sightinfo></sightinfo>
			</view>
			<!-- 收藏 -->
			<view class="collect">
				<uni-fav :star="true" :checked="false" class="favBtn" circle="true" bg-color="#ff4b3e" bg-color-checked="#007aff"
				 fg-color="#fff" @click=""></uni-fav>
				<text class="collectAttach">加入收藏后可以从我的主页查看哦</text>
			</view>
			<!-- 景点通知 tips-->
			<view class="notice">
				<text class="noticeTitle">重点通知</text>
				<text class="noticeAttach" @click="showNotice">入园须知请点击查看</text>
			</view>
			<!-- 购票入口 -->
			<view class="ticket">
				<view class="ticketBox">
					<text class="ticketTitle">门票预订</text>
				</view>
				<view class="order" @click="order">
					预订
				</view>
			</view>
			<!-- 用户评价 -->
			<view class="scoreTitle">
				<text class="scoreBox">用户评价</text>
			</view>
			<view class="sightComment">
				<scroll-view scroll-y="true" class="scroll-Y" show-scrollbar="false" @scrolltolower="showMore">
					<sight-comment v-for="(item,index) in commentNum" :key="index"></sight-comment>
					<uni-load-more status="loading"></uni-load-more>
				</scroll-view>
			</view>

		</view>

	</view>
</template>

<script>
	import sightinfo from './SightInfo.vue'
	import uniFav from '@/components/uni-fav/uni-fav.vue'
	import sightComment from '../component/sightComment.vue'
	import uniLoadMore from "@/components/uni-load-more/uni-load-more.vue"
	export default {
		components: {
			sightinfo,
			uniFav,
			sightComment,
			uniLoadMore
		},
		data() {
			return {
				sightSrc: [],
				commentNum: 5 //用户评价数量
			}
		},
		created() {
			this.getData()
		},
		methods: {
			getData() {
				this.sightSrc = [{
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
				uni.setStorageSync('imgPreviewPicList', this.sightSrc);
			},
			clickPic(index) {
				uni.setStorageSync("currentImgIndex", index);
				uni.navigateTo({
					url: '/pages/component/imgPreview'
				});
			},
			showNotice() {
				uni.navigateTo({
					url: '/pages/sightDetails/Notice'
				})
			},
			// 下拉刷新
			showMore() {
				this.commentNum += 2
			},
			// 预定
			order(){
				uni.setStorageSync("status",0);
				uni.navigateTo({
					url: '/pages/component/OrderDetails'
				})
			}
		}
	}
</script>

<style lang="less" scoped>
	.container {
		display: flex;
		flex-direction: column;
		height: calc(100vh);
		position: relative;
	}

	.swiper {
		height: 240px;
	}

	image {
		height: 100%;
		width: 100%;
	}

	.content {
		flex: 1;
		background-color: #F1F1F1;
		padding: 6px;
		position: relative;

		.infoBox {
			position: absolute;
			top: -20px;
			width: 95%;
		}

		.collect {
			margin-top: 126px;
			border-radius: 6px;
			padding: 5px;
			background-color: #fff;
			letter-spacing: 0.06em;
			font-size: 12px;
			display: flex;
			align-items: center;

			// margin-bottom: 10px;
			.collectAttach {
				margin-left: 10px;
				font-size: 10px;
			}
		}

		.notice {
			margin-top: 5px;
			border-radius: 6px;
			padding: 5px;
			background-color: #fff;
			letter-spacing: 0.06em;
			font-size: 12px;
			.noticeTitle {
				font-size: 10px;
				padding: 2px;
				border-radius: 5px;
				color: #fff;
				background-color: #ffa5d1;
				margin-right: 10px;
			}
		}

		.ticket {
			margin-top: 5px;
			border-radius: 6px;
			padding: 5px;
			background-color: #fff;
			letter-spacing: 0.06em;
			font-size: 12px;
			display: flex;
			align-items: center;
			// height: 100px;
			.ticketBox {
				flex: 1;
			}

			.ticketTitle {

				font-size: 10px;
				padding: 2px;
				border-radius: 5px;
				color: #fff;
				background-color: #ff8a53;
				margin-right: 10px;
			}

			.order {
				text-align: center;
				flex-basis: 15%;
				font-size: 20px;
				padding: 2px;
				border-radius: 5px;
				color: #fff;
				background-color: #ff6652;
				margin-right: 10px;
			}
		}

		.scoreTitle{
			margin-top: 5px;
			border-radius: 6px;
			padding: 5px;
			background-color: #fff;
			letter-spacing: 0.06em;
			font-size: 12px;
			
			.scoreBox {
				font-size: 10px;
				padding: 2px;
				border-radius: 5px;
				color: #fff;
				background-color: #86dbff;
				margin-right: 10px;
			}
		}


	}

	.scroll-Y {
		height: calc(35vh);
	}
</style>
