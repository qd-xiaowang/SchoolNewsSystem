<template>
	<div id="newscontent">
		<div v-title></div>
		<div class="page-container">
			<div class="container">
				<div class="row">
					<div class="span8 page-content">
						<article class="type-post format-standard hentry clearfix">
							<h1>
								<a>{{ content.article_title }}</a>
							</h1>
							<div class="post-meta clearfix">
								<span class="date">发布日期：{{content.article_createtime | dataFormat}}</span>
								<span class="category">
									<div>发布人：{{ content.nickname }}</div>
								</span>
							</div>
							<blockquote v-html="content.article_content"></blockquote>
						</article>
						<div class="like-btn">
							<form id="like-it-form" action="#" method="post">
								<input type="hidden" name="post_id" value="99" />
								<input type="hidden" name="action" value="like_it" />
							</form>
							<span class="tags">
								<span v-for="(tag, id) in content.help_tag" @click="$router.push(`/help/${tag}`)"
									:key="id" class="label">{{ tag }}</span>
							</span>
						</div>
						<div style='display: flex;justify-content: flex-end;'>
							<el-button type="text" @click='collection()'>点击收藏</el-button>
							<el-button type="text" @click='notcollection()'>取消收藏</el-button>
						</div>
					</div>
					<aside class="span4 page-sidebar">
						<news />
					</aside>
				</div>
			</div>
		</div>
		<!-- End of Page Container -->
	</div>
</template>

<script>
	import news from '@/components/news.vue'
	import {
		mapState,
		mapActions
	} from 'vuex'

	export default {
		name: 'newscontent',
		components: {

			news
		},
		data() {
			return {
				content: {}
			}
		},
		props: {
			id: {}
		},
		computed: {
			...mapState({
				commentnum: state => state.commentnum
			})
		},
		methods: {
			...mapActions(['setcontentid', 'setcontentinfo']),
			jubao(username) {
				let url = escape(window.location.href)
				this.$router.push({
					path: '/jubao',
					query: {
						user: username,
						url: url
					}
				})
			},
			async getarticlecontent() {
				let data = {
					id: this.id
				}
				let res = await this.$axios.post(
					'/web/getarticlecontent',
					this.qs.stringify(data)
				)
				if (res.data.state.type === 'SUCCESS') {
					this.content = res.data.data
					this.setcontentinfo({
						contentname: res.data.data.article_title,
						contentuserid: res.data.data.user_id
					})
					// this.content.help_tag = res.data.data.help_tag.split(",");
				}
			},
			// 收藏功能
			async collection(){
				let {data:{data}} = await this.$axios.post("/webadmin/getuser");
				console.log(data.nickname);
				let name = data.nickname
				if(name){
					let info = {username:data.nickname,
					article_title:this.content.article_title,
					author:this.content.nickname}
					let res = await this.$axios.post(
					'/web/collection',this.qs.stringify(info)
					)
					console.log(res)
					// console.log(this.content.nickname)
				}
				// let data = {
				// 	id: this.id
				// }
				// let res = await this.$axios.post(
				// 	'/web/getarticlecontent',
				// 	this.qs.stringify(data)
				// )
				// console.log(res)
			},
			async notcollection(){
				let {data:{data}} = await this.$axios.post("/webadmin/getuser");
				console.log(data.nickname);
				let name = data.nickname
				if(name){
					let info = {username:data.nickname,article_title:this.content.article_title}
					let res = await this.$axios.post(
					'/web/cancelcollection',this.qs.stringify(info)
					)
					console.log(res)
				}
			}
		},
		watch: {
			id: {
				handler(newVal) {
					this.getarticlecontent()
					this.setcontentid(this.id)
				},
				deep: true,
				immediate: true
			}
		},
		created() {
			// this.getarticlecontent();
			this.setcontentid(this.id);
		}
	}
</script>
<style>
	.help {
		min-height: 200px;
	}

	.label {
		margin-left: 15px;
	}
</style>
