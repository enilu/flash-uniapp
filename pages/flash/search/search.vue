<template>
	<view>
		<u-navbar :custom-back="toBack">
			<view class="slot-wrap">
				<u-search placeholder="搜热点" v-model="listQuery.keyword" :clearabled="true" @search="search" @custom="search" @clear="clear"></u-search>
			</view>
		</u-navbar>
		<u-divider v-if="noResult">发现美好生活</u-divider>
		  
	</view>
</template>

<script>
	export default {
		data() {
			return {
				keyword: '',
				noResult: true,
				goodsList: [],
				listQuery: {
					page: 1,
					limit: 20,
					keyword: ''
				},
			}
		},
		onLoad() {
			this.init();
		},
		methods: {
			init() {
				const baseApi = this.baseApi;
				this.$u.get('/goods/searchHot').then(res => {
					this.noResult = true;
					let list = res;
					for (const index in list) {
						const item = list[index];
						item.img = baseApi + '/file/getImgStream?idFile=' + item.pic;
					}
					this.goodsList = list;
				});
			},
			formatPrice(price) {
				return (price / 100).toFixed(2);
			},

			toBack() {
				this.$u.route({
					type: 'back',
					delta:1
				})
			},
			search() {
				const keyword = this.listQuery.keyword;
				if (keyword == '') {
					this.init();

				} else {

					const page = this.listQuery.page;
					const limit = this.listQuery.limit;
					const baseApi = this.baseApi;
					this.$u.get('goods/search?page=' + page + '&limit=' + limit + '&key=' + keyword).then(res => {
						let list = res.records;
						this.noResult = !list.length > 0;
						for (const index in list) {
							const item = list[index];
							item.img = baseApi + '/file/getImgStream?idFile=' + item.pic;
						}
						this.goodsList = list;
					});
				}
			},
			clear(){
				this.listQuery.keyword='';
				this.init();
			},
			toDetail(id) {
				this.$u.route({
					url: '/pages/goods/goods',
					params: {
						id: id
					}
				})
			},
		}
	}
</script>


<style scoped lang="scss">
	.slot-wrap {
		display: flex;
		align-items: center;
		/* 如果您想让slot内容占满整个导航栏的宽度 */
		flex: 1;
		/* 如果您想让slot内容与导航栏左右有空隙 */
		padding: 0 10rpx;
	}

</style>
