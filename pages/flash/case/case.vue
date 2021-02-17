<template>
	<view class="wrap">
		<u-swiper :list="banner.list"></u-swiper>
		 
		<view class="fu-product">
			<view class="fu-title">精选案例</view>
			<view class="fu-body">
				<u-grid :col="1" v-for="(item,index) in caseList" :key="index">
					<u-grid-item @click="toDetail(item.id,'case')">						 
						<view class="grid-text">{{item.name}}</view>
						<u-image width="90%" height="600rpx" :src="item.image"></u-image>
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
			toDetail(id,type) {
				this.$u.route({
					url: '/pages/flash/news/detail?id='+id+'&type='+type
				})
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
