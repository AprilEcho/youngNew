<template>
	<view class="home">
		<scroll-view class="navscroll" scroll-x>
			<view class="item" :class="index===navIndex?'active':''" v-for="(item,index) in navArr" :key="item.id"
				@click="clickNav(index,item.id)">
				{{item.classname}}
			</view>
		</scroll-view>
		<view class="content" v-if="newsArr.length">
			<view class="row" v-for="item in newsArr" :key="item.id">
				<newsbox :item="item" @click.native="goDetail"></newsbox>
			</view>
		</view>
		<view class="nodata" v-else>
			<image src="../../static/images/nodata.png" mode="widthFix"></image>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				navIndex: 0,
				navArr: [],
				newsArr: [],
				currentPage: 1
			}
		},
		onLoad() {
			this.getNavData()
			this.getNewsData()
		},
		onReachBottom() {
			this.currentPage++;
			this.getNewsData()
		},
		methods: {
			//点击导航切换
			clickNav(index, id) {
				this.navIndex = index
				//修复切换导航时页面问题
				this.currentPage = 1
				this.newsArr = []
				this.getNewsData(id)
			},
			//跳转到详情页
			goDetail() {
				uni.navigateTo({
					url: "/pages/detail/detail"
				})
			},
			//获取导航列表数据
			getNavData() {
				uni.request({
					url: "https://ku.qingnian8.com/dataApi/news/navlist.php",
					success: res => {
						this.navArr = res.data
					}
				})
			},
			//获取新闻列表
			getNewsData(id = 50) {
				uni.request({
					url: "https://ku.qingnian8.com/dataApi/news/newslist.php",
					data: {
						cid: id,
						page: this.currentPage
					},
					success: res => {
						this.newsArr = [...this.newsArr, ...res.data]
					}
				})
			}
		}
	}
</script>

<style lang="scss" scoped>
	.home {

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

		.nodata {
			padding-top: 50%;
			display: flex;
			justify-content: center;

			image {
				width: 360rpx;
			}
		}
	}
</style>