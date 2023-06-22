<template>
	<view class="detail">
		<view class="title">文章标题文章标题文章标题文章标题文章标题文章标题文章标题文章标题文章标题文章标题文章标题</view>
		<view class="info">
			<view class="author">编辑：张三</view>
			<view class="time">2022-10-10 10:10:10</view>
		</view>
		<view class="content">
			内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容
		</view>
		<view class="description">
			声明：本站的内容均采集与腾讯新闻，如果侵权请联系管理（yjs372481724@qq.com）进行整改删除，本站进行了内容采集不代表本站及作者观点，若有侵犯请及时联系管理员，谢谢您的支持。
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {

			};
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