<template>
	<view class="wrap">
		<u-swiper :list="banner.list"></u-swiper>
		<u-grid :col="5">
			<u-grid-item @click="toTab('/pages/flash/news/news')">
				<u-icon name="order" :size="46" color="#2979ff"></u-icon>
				<view class="grid-text">动态资讯</view>
			</u-grid-item>
			<u-grid-item @click="toPage('/pages/flash/product/product')">
				<u-icon name="grid-fill" :size="46" color="#2979ff"></u-icon>
				<view class="grid-text">产品服务</view>
			</u-grid-item>
			<u-grid-item @click="toPage('/pages/flash/solution/solution')">
				<u-icon name="edit-pen-fill" :size="46" color="#2979ff"></u-icon>
				<view class="grid-text">解决方案</view>
			</u-grid-item>
			<u-grid-item @click="toPage('/pages/flash/case/case')">
				<u-icon name="heart-fill" :size="46" color="#2979ff"></u-icon>
				<view class="grid-text">精选案例</view>
			</u-grid-item>
			<u-grid-item @click="scanForLogin">
				<u-icon name="phone" :size="46" color="#2979ff"></u-icon>
				<view class="grid-text">扫码登录</view>
			</u-grid-item>
		</u-grid>
		<view class="fu-product">
			<view class="fu-title">产品展示</view>
			<view class="fu-body">
				<u-grid :col="2" v-for="(item,index) in productList" :key="index" v-if="index%2 === 0">
					<u-grid-item @click="toDetail(productList[index].id,'product')">

						<view class="grid-text">{{productList[index].name}}</view>
						<u-image width="90%" height="600rpx" :src="productList[index].image"></u-image>
					</u-grid-item>
					<u-grid-item @click="toDetail(productList[index+1].id,'product')">

						<view class="grid-text">{{productList[index+1].name}}</view>
						<u-image width="90%" height="600rpx" :src="productList[index+1].image"></u-image>
					</u-grid-item>
				</u-grid>
			</view>
		</view>
		<view class="fu-solutation">
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
		<view class="fu-news">
			<view class="fu-title">热门资讯</view>
			<view class="fu-body">
				<u-row class="fu-item" v-for="(item,index) in newsList" :key="index">
					<u-col :span="3" class="fu-img">
						<u-image width="100rpx" height="100rpx" src="http://enilu.gitee.io/web-flash/logo.png" @click="toDetail(item.id)"></u-image>
					</u-col>
					<u-col :span="9" @click="toDetail(item.id,'news')">
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
					let solutionList = res.data.solutionList
					let productList = res.data.productList
					productList.forEach(function(val, index) {
						val.image = baseApi + '/file/getImgStream?idFile=' + val.img
					})
					solutionList.forEach(function(val, index) {
						val.image = baseApi + '/file/getImgStream?idFile=' + val.img
					})
					this.productList = productList
					this.solutionList = solutionList
				})
			},
			toDetail(id,type) {
				this.$u.route({
					url: '/pages/flash/news/detail?id='+id+'&type='+type
				})
			},
			toPage(url) {
				this.$u.route({
					url: url
				})
			},
			toTab(url) {
				this.$u.route({
					url: url,
					type: 'tab'
				})
			},
			scanForLogin(){
				// 只允许通过相机扫码
				uni.scanCode({
				    onlyFromCamera: true,
				    success: function (res) {
				        console.log('条码类型：' + res.scanType);
				        console.log('条码内容：' + res.result);
				    }
				});
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
