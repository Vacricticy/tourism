<template>
	<view class="container">
		<swiper class="swiper" indicator-dots="indicatorDots" autoplay="autoplay" interval="interval" duration="duration">
			<swiper-item v-for="(item,index) in sightSrcList" :key="index" @tap="clickPic(index)">
				<image :src="item" mode="aspectFill"></image>
			</swiper-item>
		</swiper>
		<view class="content">
			<!-- 景点信息 -->
			<view class="infoBox">
				<sightinfo :sightDetail="sightDetail"></sightinfo>
			</view>
			<!-- 收藏 -->
			<view class="collect">
				<uni-fav :star="true" :checked="sightDetail.keep" class="favBtn" circle="true" bg-color="#ff4b3e" bg-color-checked="#007aff"
				 fg-color="#fff" @click="collect"></uni-fav>
				<text class="collectAttach" v-if="!sightDetail.keep">加入收藏后可以从我的主页查看哦</text>
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
					<sight-comment v-for="(item,index) in userComments" :key="index" :oneComment="item"></sight-comment>
					<uni-load-more :status="isMore==0?'noMore':'loading'"></uni-load-more>
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
				currentCommentNum: 0,
				totalCommentNum: 0,
				isMore: 0, //是否存在更多评价,1表示存在
				sightSrc: [],
				userComments: [], //用户评价
				totalComments: [], //用户所有评价
				commentNum: 5, //用户评价数量
				sightId: 0,
				sightDetail: {}, //景点详情
				sightSrcList: [], //景点图片
				la: 0,
				lo: 0
			}
		},
		onLoad: function(option) { //option为object类型，会序列化上个页面传递的参数
			// console.log(option.sightId); //打印出上个页面传递的参数。
			this.sightId = option.sightId;
			this.la = option.la;
			this.lo = option.lo;
			// console.log('pppppppppppppp')
			// console.log(this.la)
		},
		created() {
			this.getData()
		},
		methods: {
			getData() {
				this.getSightDetail();
				this.getComment();
				// this.getAddressName()
			},
			//获取景点详情
			getSightDetail() {
				uni.request({
					url: 'http://117.78.2.192:8090/view/by_id',
					method: 'POST',
					data: {
						"id": this.sightId,
						"userId": uni.getStorageSync("userId")
					},
					success: (res) => {
						// console.log(res.data.status)
						if (res.data.status == 200) {
							// console.log(res.data.data[0])
							this.sightDetail = res.data.data[0];
							console.log(uni.getStorageSync("userId") + this.sightDetail.keep)
							this.sightSrcList = this.sightDetail.viewPics.split(',')							
							// console.log(this.sightSrcList)
							this.getAddressName()
						}
					}
				})

			},
			//获取景点评价
			getComment() {
				uni.request({
					url: 'http://117.78.2.192:8090/review/getReviews',
					method: 'GET',
					data: {
						viewId: this.sightId
					},
					success: (res) => {
						if (res.data.status == 200) {
							console.log(res.data.data)
							this.totalComments = res.data.data
							console.log('&&&&&&&&')
							console.log(this.totalComments)
							if (res.data.data.length > 3) {
								this.userComments = res.data.data.slice(0, 4)
								this.currentCommentNum = 3; //设置当前评价数量
								this.totalCommentNum = res.data.data.length //设置总评价数量
							} else {
								this.userComments = res.data.data;
								this.currentCommentNum = res.data.data.length; //设置当前评价数量
								this.totalCommentNum = res.data.data.length //设置总评价数量
								this.isMore = 0 //设置不存储更多数据
							}

						}
					}
				})
			},
			//获取地理位置名称
			getAddressName() {
				// console.log('lllllllaaaaaaaa')
				// console.log(this.la)
				// uni.request({ 
				// 	url: "https://restapi.amap.com/v3/geocode/regeo",
				// 	method:'GET',
				// 	data: {
				// 		key: "d99c0d2112f7f708b9a86094aca2b350",
				// 		location: this.lo + ',' + this.la,
				// 	},
				// 	success: function(data) {
				// 		console.log('mapppppppppppppppp')
				// 		console.log(data);
				// 	}
				// })
			},
			//点击查看图片详情
			clickPic(index) {
				// uni.setStorageSync("currentImgIndex", index);
				uni.navigateTo({
					url: '/pages/component/imgPreview?currentImgIndex=' + index + '&imgPreviewPicList=' + this.sightSrcList
				});
			},
			//查看景点通知
			showNotice() {
				// let url = '/pages/sightDetails/Notice?content=' + this.sightDetail.viewNotice;
				uni.navigateTo({
					url: '/pages/sightDetails/Notice?content=' + this.sightDetail.viewNotice
				})
			},
			// 下拉刷新
			showMore() {
				if (this.totalCommentNum > this.currentCommentNum) {
					// this.currentCommentNum += 2;
					// this.userComments =this.totalComments.slice(0, this.currentCommentNum)
				}
			},
			// 预定
			order() {
				uni.navigateTo({
					url: '/pages/component/OrderDetails?sightId=' + this.sightId + '&price=' + this.sightDetail.viewMoney +
						'&pictureSrc=' + this.sightSrcList[0] + '&orderStatus=-1'
				})
			},
			// 收藏功能
			collect() {
				uni.request({
					url: 'http://117.78.2.192:8090/keep',
					method: 'POST',
					data: {
						"userId": uni.getStorageSync("userId"),
						"viewId": this.sightDetail.id
					},
					success: (res) => {

					}

				})
				this.sightDetail.keep = true
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

		.scoreTitle {
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
