<template>
	<view>
		<u-swiper :list="banner.list"></u-swiper>

		<view>
			<view class="fu-title">热门资讯</view>
			<view class="fu-body">
				<u-row class="fu-item" v-for="(item,index) in newsList" :key="index">
					<u-col :span="3" class="fu-img">
						<u-image width="100rpx" height="100rpx" src="http://enilu.gitee.io/web-flash/logo.png" @click="toDetail(item.id)"></u-image>
					</u-col>
					<u-col :span="9" @click="toDetail(item.id)">
						<view class="fu-descript">{{item.desc}}</view>
					</u-col>
				</u-row>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				banner: {
					list: []
				},
				productList: [],
				solutionList: [],
				newsList: []
			}
		},
		onLoad() {
			this.init()
		},
		methods: {
			init() {
				const baseApi = this.baseApi
				this.$u.get('offcialsite').then(res => {
					console.log('res', res)
					let bannerList = res.data.banner.list

					bannerList.forEach(function(val, index) {
						val.image = baseApi + '/file/getImgStream?idFile=' + val.idFile
					})
					this.banner.list = bannerList
					this.newsList = res.data.newsList
					this.solutionList = res.data.solutionList
					this.productList = res.data.productList
				})
			},
			toDetail(url) {

			}
		}
	}
</script>

<style lang="scss" scoped>
	.fu-title {
		padding: 20rpx;
	}

	.fu-body {
		.fu-item {
			padding-top: 10rpx;

			.fu-img {
				padding: 30rpx;
			}

			.fu-name {
				font-size: 26rpx;
			}

			.fu-descript {
				font-size: 30rpx;
			}
		}

	}
</style>
