<template>
	<div class="help">
		<!-- start of page content -->
		<div style='width:1200px'>
			<span>新闻</span>
			<div tag="div" class="page-header" style="position:relative;">
			</div>
			<article class="format-standard type-post hentry clearfix" v-for="(item, id) in tableData" :key="id">
				<header class="clearfix">
					<h3>
						<router-link :to="'/newscontent/' + item.article_id">{{
              item.article_title
            }}</router-link>
					</h3>
					<div class="post-meta clearfix">
						<span class="date">发布日期：{{ item.article_createtime | dataFormat }}</span>
						<span class="category">
							<div>发布人：{{
                item.nickname
              }}</div>
						</span>
						<span class="like-count">
							热度：<!-- <a class="iconfont" style="color:red">&#xe647;</a> -->
							{{ item.article_read_num }}
						</span>
					</div>
					<!-- end of post meta -->
				</header>

			</article>
		</div>
	</div>
</template>

<script>
	export default {
		name: 'help',
		data() {
			return {
				smallttle: '',
				pagelistquery: {
					lable: '',
					tag: '',
					total: 0,
					pagesize: 3,
					page: 1
				},
				tableData: {}
			}
		},

		methods: {
			async getarticlelist() {
				let res = await this.$axios.post(
					'/web/getarticlelist',
					this.qs.stringify(this.pagelistquery)
				)
				if (res.data.state.type === 'SUCCESS') {
					this.tableData = res.data.data
					this.pagelistquery.total = res.data.count
				}
			}
		},
		created() {
			this.getarticlelist()
		}
	}
</script>
<style>
	.help {
		min-height: 200px;
	}
</style>
