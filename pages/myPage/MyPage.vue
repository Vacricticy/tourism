<template>
	<view>
		<view class="container" v-if="islogin">
			<view class="bg_img">
				<view class="myInfo">
					<image :src="url" mode=""></image>
				</view>
				<view class="nickname">
					{{nickname}}
				</view>
			</view>

			<view class="myCollect">
				<view class="collectTitle">
					<text class="titleBox">我的收藏</text>
					<text class="more" @click="more">查看更多</text>
				</view>
				<swiper class="swiper" indicator-dots="indicatorDots" autoplay="autoplay" interval="interval" duration="duration"
				 v-if="topThreeCollect.length!=0">

					<swiper-item v-for="(item,index) in topThreeCollect" :key="index">
						<sight :sight="item"></sight>
					</swiper-item>
				</swiper>
				<view class="noCollect" v-else>
					暂无收藏数据
				</view>
			</view>

			<view class="info">
				<view class="infoTitle">
					<text class="titleBox">个人信息</text>
				</view>
				<view class="infoContent">
					<view class="id xxxTitle">
						<text class="xxxtitleBox">用户帐号</text>
						<text>{{userId}}</text>
					</view>
					<view class="nickname xxxTitle">
						<text class="xxxtitleBox">修改昵称</text>
						<input class="nicknameInput" type="text" v-model="nickname" />
						<text class="xxxtitleBox confirmEdit" @click="editNickname">确认修改</text>
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
			<input class="loginInput" type="text" value="" placeholder="请输入用户账号" v-model="userId" />
			<input class="loginInput" type="password" value="" placeholder="请输入密码" v-model="password" />
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
				url: "../../static/sights/sight1.jpg", //默认背景图片
				topThreeCollect: [], //收藏列表的前三项
				allCollect: [], //所有收藏的景点
				nickname: '',
				random: 0
			}
		},
		//下拉刷新
		onLoad: function(options) {
			setTimeout(function() {
				console.log('start pulldown');
			}, 1000);
			uni.startPullDownRefresh();
		},
		onPullDownRefresh() {
			console.log('refresh');
			// 重新获取收藏列表
			this.getAllCollect()
			setTimeout(function() {
				uni.stopPullDownRefresh();
			}, 1000);
		},
		created() {
			if (uni.getStorageSync('userId')) {
				this.islogin = true
			}
			this.getAllCollect()
			this.random = Math.ceil(Math.random() * 1000);
		},
		methods: {
			myUpload(rsp) {
				this.url = rsp.path; //更新头像方式一
				console.log(this.url)
				uni.request({
					url:'http://117.78.2.192:8090/user/upload',
					// method:'PUT',
					// dataType:FormData,
					// data:{
					// 	file:this.url
					// },
					success:(res)=>{
						console.log(3333333)
						console.log(res.data)
					}
				})
				//rsp.avatar.imgSrc = rsp.path; //更新头像方式二
			},
			// 登录功能
			async login() {
				await this.doLogin();
				this.getUserInfo();
			},
			// 注册功能
			async register() {
				await this.doRegister();
				this.getUserInfo();
			},
			doLogin() {
				return new Promise((resolve, reject) => {
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
								userPwd: this.password,
								userName: 'travel@' + '17166' + this.random
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
									// uni.setStorageSync("userId", this.userId)
									// 重新获取用户收藏列表
									this.getAllCollect()
									resolve('succes');
								}

							}
						});
					}
				})
			},
			doRegister() {
				return new Promise((resolve, reject) => {
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
									// uni.setStorageSync("userId", this.userId)
									// console.log('用户id' + this.userId)
									// 重新获取用户收藏列表
									this.getAllCollect()
									resolve('succes');
								}
							}
						});
					}
				})
			},
			getUserInfo() {
				console.log('^^^^^^^^^^^^^')
				uni.request({
					url: 'http://117.78.2.192:8090/user/' + this.userId,
					method: 'GET',
					success: (res) => {
						if (res.data.status == 200) {
							console.log('bbbbbbbbbbbbbbbbb')
							console.log(res.data.data)
							uni.setStorageSync("userId", res.data.data[0].id)
							console.log('用户id' + res.data.data[0].id)
							this.nickname = res.data.data[0].userName;
							this.url=res.data.data[0].userPhoFile;
						}
					}
				})
			},

			//退出功能
			loginOut() {
				this.islogin = false
				uni.showToast({
					title: "退出成功",
					duration: 1200
				})
			},
			// 获取用户收藏列表、
			getAllCollect() {
				uni.request({
					url: 'http://117.78.2.192:8090/keep/' + uni.getStorageSync('userId'),
					success: (res) => {
						if (res.data.status == 200) {
							// console.log(res.data.data)
							this.allCollect = res.data.data;
							// console.log('allCollect:' + this.allCollect)
							this.topThreeCollect = res.data.data.splice(0, 3)
							console.log('topThreeCollect:' + this.topThreeCollect)
						}
					}
				})
			},
			// 获取所有收藏列表
			more() {
				uni.navigateTo({
					url: '/pages/myPage/Collect'
				})
			},
			// 修改昵称
			editNickname() {
				// console.log("333fffffffffffffff"+uni.getStorageSync('userId')+this.nickname)
				uni.request({
					url: 'http://117.78.2.192:8090/user',
					method: 'PUT',
					data: {
						"id": uni.getStorageSync('userId'),
						"userName": this.nickname
					},
					success: (res) => {
						// console.log(res.data)
						if (res.data.status == 200) {
							// console.log("fffffffffffffff")
							uni.showToast({
								title: '修改成功',
								duration: 1200
							})
						}
					}
				})
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
				box-shadow: 2px 2px 6px #C8C7CC;
			}

			.more {
				font-size: 18px;
				padding: 2px;
				border-radius: 5px;
				color: #fff;
				background-color: #39baff;
				box-shadow: 2px 2px 6px #C8C7CC;
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
				box-shadow: 2px 2px 6px #C8C7CC;
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
		box-shadow: 2px 2px 6px #C8C7CC;
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

	.loginInput {
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



	.nickname {
		.nicknameInput {
			height: 20px;
			width: 50%;
			border: 1px solid #F1F1F1;
			border-radius: 6px;
			box-shadow: 2px 2px 6px #F1F1F1;
			padding-left: 5px;
		}

		.confirmEdit {
			background-color: #99df60;
		}
	}

	.noCollect {
		text-align: center;
		font-size: 14px;
	}
</style>
