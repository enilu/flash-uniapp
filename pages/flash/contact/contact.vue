<template>
	<view class="wrap">
		<u-image width="100%" height="500rpx" src="/static/img/contact.png" @click="toDetail(item.id)"></u-image>
		<u-form :model="form" ref="uForm" :label-width="160">
			<u-form-item label="姓名" prop="userName">
				<u-input v-model="form.userName" placeholder="请输入姓名" />
			</u-form-item>
			<u-form-item label="手机号码" prop="mobile">
				<u-input v-model="form.mobile" placeholder="请输入手机号码" />
			</u-form-item>
			<u-form-item label="邮箱" prop="email">
				<u-input v-model="form.email" placeholder="请输入电子邮箱" />
			</u-form-item>
			<u-form-item label="留言" prop="remark">
				<u-input v-model="form.remark" type="textarea" placeholder="请输入留言" />
			</u-form-item>
			<u-button type="primary" @click="submit">提交</u-button>
		</u-form>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				form: {
					userName: '',
					mobile: '',
					email: '',
					remark: ''
				},
				rules: {
					userName: [{
						required: true,
						message: '请输入姓名',
						trigger: ['change', 'blur'],
					}],
					mobile: [{
						required: true,
						message: '请输入手机号码',
						trigger: ['change', 'blur'],
					}],
					email: [{
						required: true,
						message: '请输入邮箱',
						trigger: ['change', 'blur'],
					}],
					remark: [{
						required: true,
						message: '请输入留言',
						trigger: ['change', 'blur'],
					}],

				}
			}
		},
		onReady() {
			this.$refs.uForm.setRules(this.rules)
		},
		methods: {
			submit() {

				this.$refs.uForm.validate(valid => {
					if (valid) {
						this.$u.post('/offcialsite/contact',this.form).then(res => {
							this.$u.toast('提交成功')
							this.form={}
						})
					}
				})

			}
		}
	}
</script>

<style lang="scss" scoped>
	.wrap {
		font-size: 28rpx;
		padding: 30rpx;
	}
</style>
