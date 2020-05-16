<template>
	<view>
		<view class="commentBox">
			<view class="head">
				<view class="avatar">
					<image :src="oneComment.userPhoFile" mode=""></image>
				</view>
				<view class="name_score">
					<text class="name">{{oneComment.userName}}</text>
					<view class="score">
						<text>打分</text><uni-rate :value="oneComment.reviewScore" disabled size="12"></uni-rate>
					</view>
				</view>
				<view class="date">
					{{oneComment.trueTime}}
				</view>
			</view>
			<view class="content">
				{{oneComment.reviewContent}}
			</view>
		</view>
	</view>
</template>

<script>
	import uniRate from '../../components/uni-rate/uni-rate.vue'
	export default {
		props:[
			'oneComment'
		],
		components:{
			uniRate
		},
		data() {
			return {
				
			}
		},
		methods: {

		}
	}
</script>

<style lang="less" scoped>
	.commentBox {
		margin-top: 10px;
		background-color: #fff;
		border-radius: 6px;
		padding: 10px;
		font-size: 12px;
	}

	.head {
		display: flex;

		.avatar {
			width: 40px;
			height: 40px;
			border-radius: 50%;
			box-shadow: 0 0 10px #b3b3b6;
			margin-right: 10px;

			image {
				width: 100%;
				height: 100%;
				border-radius: 50%;
			}
		}

		.name_score {
			flex: 1;

			.name {
				font-weight: bold;
				font-size: 14px;
			}
			.score{
				margin-top: 8px;
				display: flex;
				line-height: 3px;
				font-size: 10px;
				text{
					margin-right: 6px;			
				}
			}
		}

		.date {
			flex-basis: 15%;
		}
	}

	.content {
		margin-top: 10px;
	}
</style>
