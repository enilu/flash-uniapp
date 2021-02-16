<template>
	<view>
		<u-swiper :list="banner.list"></u-swiper>
		<u-grid :col="5">
			<u-grid-item>
				<u-icon name="order" :size="46" color="#2979ff"></u-icon>
				<view class="grid-text">动态资讯</view>
			</u-grid-item>
			<u-grid-item>
				<u-icon name="grid-fill" :size="46" color="#2979ff"></u-icon>
				<view class="grid-text">产品服务</view>
			</u-grid-item>
			<u-grid-item>
				<u-icon name="edit-pen-fill" :size="46" color="#2979ff"></u-icon>
				<view class="grid-text">解决方案</view>
			</u-grid-item>
			<u-grid-item>
				<u-icon name="heart-fill" :size="46" color="#2979ff"></u-icon>
				<view class="grid-text">精选案例</view>
			</u-grid-item>
			<u-grid-item>
				<u-icon name="phone" :size="46" color="#2979ff"></u-icon>
				<view class="grid-text">立即咨询</view>
			</u-grid-item>
		</u-grid>
		<view class="fu-product">
			<view class="fu-title">产品展示</view>
			<view class="fu-body">
				<u-grid :col="2" v-for="(item,index) in productList" :key="index" v-if="index%2 === 0">
					<u-grid-item>
						 
						<view class="grid-text">{{productList[index].name}}</view>
						<u-image width="100%" height="300rpx" :src="productList[index].img"></u-image>
					</u-grid-item>
					<u-grid-item>
					 
						<view class="grid-text">{{productList[index+1].name}}</view>
						<u-image width="100%" height="300rpx" :src="productList[index+1].img"></u-image>
					</u-grid-item>
				</u-grid>
			</view>
		</view>
		<view class="fu-news">
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
