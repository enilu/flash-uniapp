<template>
	<view class="wrap">
		<u-swiper :list="banner.list"></u-swiper>
		 
		<view class="fu-product">
			<view class="fu-title">解决方案</view>
			<view class="fu-body">
				<u-grid :col="2" v-for="(item,index) in solutionList" :key="index" v-if="index%2 === 0">
					<u-grid-item  @click="toDetail(solutionList[index].id,'solution')">
						 
						<view class="grid-text">{{solutionList[index].name}}</view>
						<u-image width="90%" height="600rpx" :src="solutionList[index].image"></u-image>
					</u-grid-item>
					<u-grid-item  @click="toDetail(solutionList[index+1].id,'solution')">
					 
						<view class="grid-text">{{solutionList[index+1].name}}</view>
						<u-image width="90%" height="600rpx" :src="solutionList[index+1].image"></u-image>
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
				solutionList: [], 
			}
		},
		onLoad() {
			this.init()
		},
		methods: {
			init() {
				const baseApi = this.baseApi
				this.$u.get('offcialsite/solution').then(res => {
					 
					let bannerList = res.data.banner.list

					bannerList.forEach(function(val, index) {
						val.image = baseApi + '/file/getImgStream?idFile=' + val.idFile
					})
					this.banner.list = bannerList
				 
					let solutionList = res.data.solutionList
					solutionList.forEach(function(val, index) {
						val.image = baseApi + '/file/getImgStream?idFile=' + val.img
					})
				  
					this.solutionList = solutionList 
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
