<template>
	<view class="user">
		<view class="top">
			<image src="../../static/images/history.png" mode=""></image>
			<view class="text">浏览历史</view>
		</view>
		<view class="content" v-if="listArr.length">
			<view class="row" v-for="item in listArr">
				<newsbox :item="item" @click.native="goDetail(item)"></newsbox>
			</view>
		</view>
		<view class="nohistory" v-else>
			<image src="../../static/images/nohis.png" mode="widthFix"></image>
			<text class="text">暂无浏览记录</text>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				listArr: []
			};
		},
		onShow() {
			this.getData()
		},
		methods: {
			//跳转到详情页
			goDetail(item) {
				uni.navigateTo({
					url: `/pages/detail/detail?cid=${item.classid}&id=${item.id}`
				})
			},
			//获取缓存浏览记录
			getData() {
				let hisArr = uni.getStorageSync("historyArr") || []
				this.listArr = hisArr
			}
		}
	}
</script>

<style lang="scss">
	.user {
		.top {
			padding: 50rpx 0;
			background: #f8f8f8;
			color: #666;
			display: flex;
			flex-direction: column;
			justify-content: center;
			align-items: center;

			image {
				width: 150rpx;
				height: 150rpx;
			}

			.text {
				font-size: 38rpx;
				padding-top: 20rpx;
			}
		}

		.content {
			padding: 30rpx;

			.row {
				border-bottom: 1rpx dashed #efefef;
				padding: 20rpx 0;
			}
		}

		.nohistory {
			display: flex;
			flex-direction: column;
			justify-content: center;
			align-items: center;

			image {
				width: 450rpx;
			}

			.text {
				font-size: 26rpx;
				color: #888;
			}
		}
	}
</style>