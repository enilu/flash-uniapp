<template>
	<view class="wrap">
		<view class="u-m-t-20">
			<u-cell-group>
				<u-cell-item icon="camera" title="头像" @click="openPage('/pages/flash/setting/updateAvatar')">
					<u-image slot="right-icon" width="80rpx" height="80rpx" :src="vuex_avatar"></u-image>
				</u-cell-item>
				<u-cell-item icon="account" title="姓名" @click="updateNickName" :value="user.nickName"></u-cell-item>
				<u-cell-item :icon="user.genderIcon?user.genderIcon:'man'" title="性别" @click="updateGender" :value="user.genderStr"></u-cell-item>
			</u-cell-group>
		</view>
		<view class="u-m-t-20">
			<u-cell-group>
				<u-cell-item icon="lock" title="修改密码" @click="openPage('/pages/flash/setting/updatePwd')"></u-cell-item>
				<u-cell-item icon="phone" title="更换手机" @click="todo('/pages/flash/setting/updateMobile')" :value="user.mobile"></u-cell-item>

			</u-cell-group>
		</view>

		<view class="u-m-t-20">
			<u-cell-group>
				<u-cell-item icon="info-circle" title="关于我们" @click="openPageWithNoLogin('/pages/flash/setting/about')"></u-cell-item>
				<u-cell-item icon="file-text" title="在线文档" @click="openPageWithNoLogin('/pages/flash/setting/document')"></u-cell-item>

			</u-cell-group>
		</view>
		<view class="u-m-t-20">
			<u-button @click="logout">退出</u-button>
		</view>

		<u-modal v-model="nickName.show" title="修改用户名" @confirm="submitNickName" :show-cancel-button="true">
			<view style="padding:10px 50px;">
				<u-input style="padding: 0rpx 30rpx;background-color: #ededed;" v-model="nickName.value" placeholder="请输入用户名" />
			</view>
		</u-modal>

		<u-action-sheet :list="gender.list" v-model="gender.show" :cancel-btn="false" @click="submitGender"></u-action-sheet>


	</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				isLogin: false,
				user: {
					genderIcon: 'man',
				},
				gender: {
					show: false,
					list: [{
						text: '男',
						value:'male'
					}, {
						text: '女',
						value:'female'
					}]
				},
				nickName: {
					show: false,
					value: ''
				}
			}
		},
		onLoad() {
			this.init();
		},
		methods: {
			init() {
				const user = this.vuex_user;
				if (user.nickName !== '未登录') {
					if (user.gender == 'female') {
						user.genderStr = '女';
						user.genderIcon = 'woman';
					}

					if (user.gender == 'male') {
						user.genderStr = '男';

						user.genderIcon = 'man';
					}

					this.user = user;

				} else {
					this.user = user;
				}
			},
			openPageWithNoLogin(url){
				this.$u.route({
					url: url
				})
			},
			openPage(url) {
				if (this.user.nickName == '未登录') {
					this.$u.route('/pages/flash/login/login');
					return;
				}				
				this.$u.route({
					url: url
				})
			},
			todo(url){
				this.$u.toast('开发中');
			},
			updateNickName() {

				this.nickName.value = this.user.nickName;
				this.nickName.show = true;
			},
			submitNickName() {
				this.$u.post('user/updateUserName/' + this.nickName.value).then(res => {
					let user = this.vuex_user;
					user.nickName = this.nickName.value;
					this.$u.vuex('vuex_user', user);
					this.$u.toast('修改成功');
				});
			},
			updateGender() {
				this.gender.show = true;
			},
			submitGender(index) {
				let selData = this.gender.list[index];
				this.$u.post('user/updateGender/' + selData.value).then(res => {
					let user = this.vuex_user;
					user.gender = selData.value;
					if (user.gender == 'female') {
						this.user.genderStr = '女';
						this.user.genderIcon = 'woman';
					}
					
					if (user.gender == 'male') {
						this.user.genderStr = '男';					
						this.user.genderIcon = 'man';
					}
					console.log(this.user);
					
					this.$u.vuex('vuex_user', user);
					this.$u.toast('修改成功');
				});
			},
			logout() {
				this.$u.vuex('vuex_token', '');
				this.$u.vuex('vuex_user', {
					name: '未登录'
				});
				this.$u.route('/pages/flash/login/login')
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
	.wrap {
		.bottom {
			position: fixed;
			bottom: 0;
			width: 100%;
		}
	}
</style>
