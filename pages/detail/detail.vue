<template>
	<view class="detail">
		<view class="title">{{detail.title}}</view>
		<view class="info">
			<view class="author">编辑：{{detail.author}}</view>
			<view class="time">发布日期：{{detail.posttime}}</view>
		</view>
		<view class="content">
			<rich-text :nodes="detail.content"></rich-text>
		</view>
		<view class="description">
			声明：本站的内容均采集与腾讯新闻，如果侵权请联系管理（yjs372481724@qq.com）进行整改删除，本站进行了内容采集不代表本站及作者观点，若有侵犯请及时联系管理员，谢谢您的支持。
		</view>
	</view>
</template>

<script>
	import {
		parseTime
	} from '@/utils/tool.js'
	export default {
		data() {
			return {
				options: null,
				detail: {}
			};
		},
		onLoad(e) {
			this.options = e
			this.getDetail()
		},
		methods: {
			getDetail() {
				uni.request({
					url: "https://ku.qingnian8.com/dataApi/news/detail.php",
					data: this.options,
					success: res => {
						res.data.posttime = parseTime(res.data.posttime)
						res.data.content = res.data.content.replace ? res.data.content.replace(/<img/gi,
							'<img style="max-width:100%"') : res.data.content
						this.detail = res.data
						uni.setNavigationBarTitle({
							title: this.detail.title
						})
						this.saveHistory()
					}
				})
			},
			//存储个人浏览记录
			saveHistory() {
				let {
					id,
					classid,
					picurl,
					title
				} = this.detail
				let historyArr = uni.getStorageSync("historyArr") || []
				let item = {
					id,
					classid,
					picurl,
					title,
					looktime: parseTime(Date.now())

				}
				let index = historyArr.findIndex(i => {
					return i.id === item.id
				})
				if (index === -1) {
					historyArr.unshift(item)
					historyArr = historyArr.slice(0, 10)
					uni.setStorageSync("historyArr", historyArr)
				}
			}
		}
	}
</script>

<style lang="scss">
	.detail {
		padding: 30rpx;

		.title {
			font-size: 46rpx;
			color: #333;
		}

		.info {
			background: #F6F6F6;
			padding: 20rpx;
			font-size: 25rpx;
			color: #666;
			display: flex;
			justify-content: space-between;
			margin: 30rpx 0;
		}

		.content {
			padding-bottom: 30rpx;
		}

		.description {
			background: #FEF0F0;
			font-size: 26rpx;
			padding: 20rpx;
			color: #F89898;
			line-height: 1.8em;
		}
	}
</style>