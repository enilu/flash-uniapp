<template>
	<view class="wrap">
		<view class="title">{{detail.title}}</view>
		<view class="header">
			<u-row>
				 
				<u-col :span="12">
					{{detail.author}}
					{{detail.createTime}}
				</u-col>

			</u-row>
		</view>
		<view class="body">
			<u-parse :html="detail.content"></u-parse>
		</view>


	</view>
</template>

<script>
	export default {
		data() {
			return {
				id: '',
				type: '',
				detail: ''
			}
		},
		onLoad(option) {
			this.id = option.id
			this.type = option.type
			this.init()
		},
		methods: {
			init() {
				const baseApi = this.baseApi
				this.$u.get('offcialsite/article?id=' + this.id + '&type=' + this.type).then(res => { 
					this.detail = res
				})
			}
		}
	}
</script>

<style lang="scss" scoped>
	.wrap{
		padding:30rpx;
	}
	.title{
		font-size:34rpx;
	}
	.header{
		font-size:26rpx;
		margin:30rpx 0rpx;
	} 
	
</style>
