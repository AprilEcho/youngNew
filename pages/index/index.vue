<template>
	<view class="home">
		<scroll-view class="navscroll" scroll-x>
			<view class="item" :class="index===navIndex?'active':''" v-for="(item,index) in 10" @click="clickNav(index)">国内
			</view>
		</scroll-view>
		<view class="content">
			<view class="row" v-for="item in 10">
				<newsbox @click.native="goDetail"></newsbox>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				navIndex: 0
			}
		},
		onLoad() {

		},
		methods: {
			//点击导航切换
			clickNav(index) {
				this.navIndex = index
			},
			//跳转到详情页
			goDetail() {
				uni.navigateTo({
					url: "/pages/detail/detail"
				})
			}
		}
	}
</script>

<style lang="scss" scoped>
	.navscroll {
		height: 100rpx;
		white-space: nowrap;
		background-color: #F7F8FA;
		position: fixed;
		top: var(--window-top);
		left: 0;
		z-index: 99;

		/deep/ ::-webkit-scrollbar {
			width: 4px !important;
			height: 1px !important;
			overflow: auto !important;
			background: transparent !important;
			-webkit-appearance: auto !important;
			display: block;
		}

		.item {
			font-size: 40rpx;
			display: inline-block;
			line-height: 100rpx;
			padding: 0 30rpx;
			color: #333;

			&.active {
				color: #31C27C;
			}
		}
	}

	.content {
		padding: 30rpx;
		padding-top: 100rpx;

		.row {
			border-bottom: 1rpx dashed #efefef;
			padding: 20rpx 0;
		}
	}
</style>