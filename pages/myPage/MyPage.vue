<template>
	<view>
		<view class="container" v-if="islogin">
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
						<avatar selWidth="200px" selHeight="400upx" @upload="myUpload" :avatarSrc="url" avatarStyle="width: 200upx; height: 200upx; border-radius: 100%;">
						</avatar>
					</view>
					<view class="loginOutBox xxxTitle">
						<text class="loginOut xxxtitleBox" @click="loginOut">退出登录</text>
					</view>
				</view>

			</view>
		</view>
		<view class="loginBox" v-else>
			<view class="imageBox">
				<image src="../../static/logo3.png" mode=""></image>
			</view>
			<input type="text" value="" placeholder="请输入用户账号" v-model="userId" />
			<input type="password" value="" placeholder="请输入密码" v-model="password" />
			<view class="loginButton">
				<text @click="login">登录</text>
				<text @click="register">注册</text>
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
				islogin: false,
				userId: '',
				password: '',
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
		created(){
			if(uni.getStorageSync('userId')){
				this.islogin=true
			}
		},
		methods: {
			more() {
				uni.navigateTo({
					url: '/pages/myPage/Collect'
				})
			},
			myUpload(rsp) {
				this.url = rsp.path; //更新头像方式一
				console.log(this.url)
				//rsp.avatar.imgSrc = rsp.path; //更新头像方式二
			},
			// 登录功能
			login() {
				if (this.userId.trim() == '' || this.password.trim() == '') {
					uni.showToast({
						title: '输入不能为空',
						duration: 1000
					})
				} else {
					uni.request({
						url: 'http://117.78.2.192:8090/user/login',
						method: 'POST',
						data: {
							userNumber: this.userId,
							userPwd: this.password
						},
						success: (res) => {
							console.log(res.data.status)
							if (res.data.status != 200) {
								uni.showToast({
									title: '账号或密码有误',
									duration: 1200
								})
							} else {
								this.islogin = true;
								uni.setStorageSync("userId", this.userId)
							}
						}
					});
				}
			},
			// 注册功能
			register() {
				if (this.userId.trim() == '' || this.password.trim() == '') {
					uni.showToast({
						title: '输入不能为空',
						duration: 1000
					})
				} else {
					uni.request({
						url: 'http://117.78.2.192:8090/user/register',
						method: 'POST',
						data: {
							userNumber: this.userId,
							userPwd: this.password
						},
						success: (res) => {
							// console.log(res.data.status)
							if (res.data.status != 200) {
								uni.showToast({
									title: '账号已被注册',
									duration: 1200
								})
							} else {
								uni.showToast({
									title: '注册成功',
									duration: 1200
								})
								this.islogin = true;
								uni.setStorageSync("userId", this.userId)
								console.log('用户id'+this.userId)
							}
						}
					});
				}
			},
			//退出功能
			loginOut() {
				this.islogin = false
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
		height: 340rpx;
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
				background-color: #ff6f6f;
				margin-right: 10px;
				font-weight: bold;
			}

			.more {
				font-size: 18px;
				padding: 2px;
				border-radius: 5px;
				color: #fff;
				background-color: #39baff;
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
				background-color: #99df60;
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
		background-color: #39baff;
	}

	.loginBox {
		display: flex;
		justify-content: center;
		align-items: center;
		flex-direction: column;
		height: calc(100vh);
		background-color: #F1F1F1;
	}

	.imageBox {
		width: 100px;
		height: 100px;
		border: 1px solid #F1F1F1;
		border-radius: 50%;
		padding: 5px;
		box-shadow: 0 0 10px #C8C7CC;
		background-color: #fff;
		margin-bottom: 20px;

		image {
			width: 100%;
			height: 100%;
			border-radius: 50%;
		}
	}

	input {
		margin: 10px;
		border: 1px solid #C8C7CC;
		border-radius: 10px;
		box-shadow: 2px 2px 10px #C8C7CC;
		width: 80%;
		height: 55px;
		background-color: #fff;
		padding-left: 5px;
	}

	.loginButton {
		margin-top: 20px;
		width: 80%;
		display: flex;
		justify-content: space-around;

		text {
			text-align: center;
			width: 120px;
			color: #fff;
			background-color: #9ac756;
			border-radius: 5px;
			box-shadow: 3px 3px 8px #aaaaaa;
			// margin-left: 10px;
			// margin-right: 10px;
		}
	}

	.loginOut {
		background-color: #ffb253;
	}
</style>
