<template>
	<view class="wrap">
		<view class="content">
			<view class="title">密码登录</view>
			<u-form-item>
				<u-input type="string" v-model="account" placeholder="请输入手机号/账号" />
			</u-form-item>
			<u-form-item>
				<u-input type="password" v-model="password" placeholder="请输入密码" />
			</u-form-item>
			<button @tap="submit" :style="[inputStyle]" class="getSmsCode">登录</button>
			<view class="alternative">
				<view class="password" @click="goPage('/pages/flash/login/login')">手机号登录</view>
				<view class="issue">遇到问题</view>
			</view>
		</view>
		<view class="bottom">
			<view class="hint">
				登录代表同意
				<text class="link" @click="goPage('/pages/flash/login/userProtocol')">用户协议</text>、
				<text class="link" @click="goPage('/pages/flash/login/privateProtocol')">隐私政策，</text>
				并授权使用您的账号信息（如昵称、头像、收获地址）以便您统一管理
			</view>
		</view>

	</view>
</template>

<script>
	export default {
		data() {
			return {
				account: '',
				password: ''
			}
		},
		computed: {
			inputStyle() {
				let style = {};
				if (this.account) {
					style.color = "#fff";
					style.backgroundColor = this.$u.color['warning'];
				}
				return style;
			}
		},
		methods: {
			submit() {
				this.$u.post('account/login?username=' + this.account + '&password=' + this.password).then(res => {
					this.$u.vuex('vuex_token', res.token)
					this.$u.get('account/info').then(res2 => {
						this.$u.vuex('vuex_user', res2.profile)
						if (res2.profile && res2.profile.avatar !== '') {
							this.$u.vuex('vuex_avatar', this.baseApi + '/file/getImgStream?idFile=' + res2.profile.avatar);
						}
						this.$u.route({
							type: 'switchTab',
							url: '/pages/flash/user/user'
						})
					})
				}).catch(res => {
					this.$u.toast(res.msg);
				})
			},
			goPage(url) {
				this.$u.route({
					url: url
				})
			}
		}
	};
</script>

<style lang="scss" scoped>
	.wrap {
		font-size: 28rpx;

		.content {
			width: 600rpx;
			margin: 0rpx auto;
			padding-top: 80rpx;

			.title {
				text-align: left;
				font-size: 60rpx;
				font-weight: 500;
				margin-bottom: 100rpx;
			}

			input {
				text-align: left;
				margin-bottom: 10rpx;
				padding-bottom: 6rpx;
			}

			.tips {
				color: $u-type-info;
				margin-bottom: 60rpx;
				margin-top: 8rpx;
			}

			.getSmsCode {
				margin-top: 30rpx;
				background-color: rgb(253, 243, 208);
				color: $u-tips-color;
				border: none;
				font-size: 30rpx;
				padding: 12rpx 0;

				&::after {
					border: none;
				}
			}

			.alternative {
				color: $u-tips-color;
				display: flex;
				justify-content: space-between;
				margin-top: 30rpx;
			}
		}

		.bottom {
			position: absolute;
			bottom: 120rpx;

			.hint {
				padding: 20rpx 40rpx;
				font-size: 20rpx;
				color: $u-tips-color;

				.link {
					color: $u-type-warning;
				}
			}
		}

	}
</style>
