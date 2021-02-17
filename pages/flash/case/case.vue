<template>
	<view class="wrap">
		<u-swiper :list="banner.list"></u-swiper>
		 
		<view class="fu-product">
			<view class="fu-title">精选案例</view>
			<view class="fu-body">
				<u-grid :col="2" v-for="(item,index) in caseList" :key="index" v-if="index%2 === 0">
					<u-grid-item>
						 
						<view class="grid-text">{{caseList[index].name}}</view>
						<u-image width="90%" height="600rpx" :src="caseList[index].image"></u-image>
					</u-grid-item>
					<u-grid-item>
					 
						<view class="grid-text">{{caseList[index+1].name}}</view>
						<u-image width="90%" height="600rpx" :src="caseList[index+1].image"></u-image>
					</u-grid-item>
				</u-grid>
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
				caseList: [], 
			}
		},
		onLoad() {
			this.init()
		},
		methods: {
			init() {
				const baseApi = this.baseApi
				this.$u.get('offcialsite/case').then(res => {
					 
					let bannerList = res.data.banner.list

					bannerList.forEach(function(val, index) {
						val.image = baseApi + '/file/getImgStream?idFile=' + val.idFile
					})
					this.banner.list = bannerList
				 
					let caseList = res.data.caseList
					caseList.forEach(function(val, index) {
						val.image = baseApi + '/file/getImgStream?idFile=' + val.img
					})
				  
					this.caseList = caseList 
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
