<template>
	<view class="wrap">
		<view class="u-flex user-box u-p-l-30 u-p-r-20 u-p-b-30">
			<view class="u-m-r-10">
				<u-avatar :src="vuex_avatar" size="140"></u-avatar>
			</view>
			<view class="u-flex-1">
				<view class="u-font-18 u-p-b-20">{{vuex_user.name}}</view>
				<view class="u-font-14 u-tips-color" v-if="vuex_user.name !== '未登录'">ID:{{vuex_user.phone}}</view>
				<view class="u-font-14 u-tips-color" v-if="vuex_user.name == '未登录'" @click="toLogin">点击登录flash-uniapp账号</view>
			</view>
			<!-- <view class="u-m-l-10 u-p-10">
				<u-icon name="scan" color="#969799" size="28"></u-icon>
			</view> -->
			<!-- <view class="u-m-l-10 u-p-10">
				<u-icon name="arrow-right" color="#969799" size="28"></u-icon>
			</view> -->
		</view>

		<view class="u-m-t-20">
			<u-cell-group>
				<u-cell-item icon="heart" title="我的收藏" @click="openPage('/pages/order/list')"></u-cell-item>
			</u-cell-group>
		</view>

		<view class="u-m-t-20">
			<u-cell-group>
				<u-cell-item icon="grid" title="组件" @click="openPage('/pages/example/components')"></u-cell-item>
				<u-cell-item icon="setting" @click="openPage('/pages/example/js')" title="工具"></u-cell-item>
				<u-cell-item icon="grid-fill" @click="openPage('/pages/example/template')" title="模板"></u-cell-item> 
			</u-cell-group>
		</view>

		<view class="u-m-t-20">
			<u-cell-group>
				<u-cell-item icon="setting" title="设置" @click="openPage('/pages/flash/setting/setting')"></u-cell-item>
				<!-- #ifdef H5 -->
				<u-cell-item icon="download" title="下载APP" @click="openPage('/pages/flash/setting/app')"></u-cell-item>
				<!-- #endif -->
			</u-cell-group>
		</view>

	</view>
</template>

<script>
	export default {
		data() {
			return {
				show: true
			}
		},
		onLoad() {
			console.log('load.......')
		},
		onShow() {
			// #ifdef H5
			this.initH5()
			// #endif
		},
		methods: {
			initH5() {
				
				if (this.vuex_user.name !=='未登录' && this.vuex_user.refreshWechatInfo === true) {
					const url = window.location.href
					if (url.indexOf('localhost') > -1 || url.indexOf('127.0.0.1') > -1) {
					} else {
						const userAgent = window.navigator.userAgent.toLowerCase()
						//使用微信访问本系统的时候获取微信openid，否则不获取
						if (userAgent.indexOf('micromessenger') > -1) {
							this.processOpenid()
						}
					}
				}
				
			},
			openPage(url) {
				this.$u.route({
					url: url
				})
			},
			toLogin() {
				this.$u.route({
					url: '/pages/flash/login/passwordLogin'
				})
			},
			todo() {
				this.$u.toast('开发中')
			},
			processOpenid() {
				let url = window.location.href;
				if (url.indexOf('code') > -1) {
					const code = url.split('code=')[1].split("&")[0];
					this.$u.post('wechat/getWxOpenId?code='+code).then(res => {
						this.$u.vuex('vuex_token', res.token)
						this.$u.vuex('vuex_user', res.user)
						//优先展示手动上传的头像
						if (res.user.avatar && res.user.avatar !== '') {
							this.$u.vuex('vuex_avatar', this.baseApi + '/file/getImgStream?idFile=' + res.user.avatar);
						} else {
							if (res.user.wechatHeadImgUrl && res.user.wechatHeadImgUrl !== '') {
								//如果拉取到用户微信头像，则展示微信头像
								this.$u.vuex('vuex_avatar', res.user.wechatHeadImgUrl)
							}
						}
					})
				} else {
					this.redirectForCode()
				}
			},
			redirectForCode() {
				this.$u.post('wechat/getWxSign?url='+window.location.href).then(res => {
					const rr = res;
					const redirectUrl = window.location.href;
					let param = 'appid=' + rr.appId
					param += '&response_type=code'
					param += '&scope=snsapi_base'
					param += '&redirect_uri=' + encodeURIComponent(redirectUrl)
					param += '&state=linjiashop#wechat_redirect' 
					window.location.href = 'https://open.weixin.qq.com/connect/oauth2/authorize?' + param
				})
			}
		}
	}
</script>

<style>
	page {
		background-color: #ededed;
	}
</style>

<style lang="scss" scoped>
	.camera {
		width: 54px;
		height: 44px;

		&:active {
			background-color: #ededed;
		}
	}

	.user-box {
		background-color: #fff;
	}
</style>
