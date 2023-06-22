<template>
	<view class="home">
		<scroll-view class="navscroll" scroll-x>
			<view class="item" :class="index===navIndex?'active':''" v-for="(item,index) in navArr" :key="item.id"
				@click="clickNav(index,item.id)">
				{{item.classname}}
			</view>
		</scroll-view>
		<view class="content">
			<view class="row" v-for="item in newsArr" :key="item.id">
				<newsbox :item="item" @click.native="goDetail(item)"></newsbox>
			</view>
		</view>
		<view class="nodata" v-if="!newsArr.length">
			<image src="../../static/images/nodata.png" mode="widthFix"></image>
		</view>
		<view class="loading" v-if="newsArr.length">
			<view v-if="loading===1">数据加载中...</view>
			<view v-if="loading===2">没有更多数据啦~</view>
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
				currentPage: 1,
				loading: 0, //0默认 1加载中 2没有更多了
			}
		},
		onLoad() {
			this.getNavData()
			this.getNewsData()
		},
		onReachBottom() {
			//没有更多数据时不发送请求
			if (this.loading === 2) {
				return
			}
			this.currentPage++;
			this.loading = 1;
			this.getNewsData()
		},
		methods: {
			//点击导航切换
			clickNav(index, id) {
				this.navIndex = index
				//修复切换导航时页面问题
				this.currentPage = 1
				this.newsArr = []
				this.loading = 0;
				this.getNewsData(id)
			},
			//跳转到详情页
			goDetail(item) {
				uni.navigateTo({
					url: `/pages/detail/detail?cid=${item.classid}&id=${item.id}`
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
						if (res.data.length === 0) {
							this.loading = 2
							return
						}
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

		.loading {
			text-align: center;
			font-size: 26rpx;
			color: #888;
			line-height: 2em;
			padding-bottom: 1em;
		}
	}
</style>