<template>
	<view>
		<view class="commentBox">
			<view class="head">
				<view class="avatar">
					<image src="../../static/sights/sight3.jpg" mode=""></image>
				</view>
				<view class="name_score">
					<text class="name">听雨小秋</text>
					<view class="score">
						<text>打分</text><uni-rate value="3.9" disabled size="12"></uni-rate>
					</view>
				</view>
				<view class="date">
					5月2日
				</view>
			</view>
			<view class="content">
				{{content}}
			</view>
		</view>
	</view>
</template>

<script>
	import uniRate from '../../components/uni-rate/uni-rate.vue'
	export default {
		components:{
			uniRate
		},
		data() {
			return {
				content: '淡黄的长裙,蓬松的头发,牵着我的手看最新展出的油画,无人的街道,在空荡的家里,就只剩我一个人狂欢的party,就当是一场梦,醒了还是很感动,还是很想被你保护我心里的惨痛,喜欢我很辛苦,其实我都清楚,放心这世界很大我记得你的叮嘱,'
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
		
	}
</style>
