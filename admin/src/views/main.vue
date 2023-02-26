<template>
	<div class="main">
		<el-container style="height: 100vh;">
			<div class="aside" width="200px" style="background-color: #4a7cdd">
				<el-menu router default-active="2" class="el-menu-vertical-demo" background-color="#4a7cdd"
					text-color="#fff" active-text-color="#ffd04b">
					<el-menu-item style="  color: white; font-size:20px">
						<span slot="title" style="font-size:14px">校园新闻系统</span>
					</el-menu-item>
					<el-menu-item index="/contentexamine">
						<template slot="title" :disabled="uinfo.jurisdiction.issh !=='1'&& uinfo.username !== 'admin'">
							<span slot="title">内容审核</span>
						</template>
					</el-menu-item>
					<el-submenu index="2" :disabled="uinfo.jurisdiction.isyh !=='1'&& uinfo.username !== 'admin'">
						<template slot="title">
							<span slot="title">人员管理</span>
						</template>
						<el-menu-item index="/useruser">学生管理</el-menu-item>
						<el-menu-item index="/useradmin" :disabled="uinfo.username !== 'admin'">管理员管理</el-menu-item>
					</el-submenu>
					<el-menu-item index="/managementlable">新闻分类管理</el-menu-item>
					<el-menu-item index="/">
						<span slot="title">管理员信息设置</span>
					</el-menu-item>
					
				</el-menu>
			</div>
			<el-container style=" position:relative ;">
				<el-header>
					<div class="top-content">
						<div></div>
						<span @click="withdraw" style="cursor: pointer;">退出登录</span>
					</div>
				</el-header>
				<el-main style=" padding: 0px; background-color:#fdfcf8;">
					<router-view />
				</el-main>
			</el-container>
		</el-container>
	</div>
</template>



<script>
	import {
		mapState,
		mapActions
	} from "vuex";

	export default {
		data() {
			const item = {
				date: "",
				name: "",
				address: " "
			};
			return {
				tableData: Array(20).fill(item),
				isCollapse: true
			};
		},
		computed: {
			...mapState({
				uinfo: state => state.user.uinfo,
			})
		},
		methods: {
			...mapActions("user", [
				"deleteuserinfo",
			]),
			withdraw() {
				this.$router.push('/login');
				this.deleteuserinfo()
			},
			changeCollapse() {
				this.isCollapse = !this.isCollapse;
			}
		}
	};
</script>
<style>
	.el-menu-vertical-demo:not(.el-menu--collapse) {
		width: 200px;
	}

	.el-header {
		text-align: right;
		font-size: 12px;
		background-color: #4a7cdd;
		color: white;
		line-height: 56px;
		height: 56px;
		text-align: right;
		padding: 0px 30px;
	}

	.el-menu {
		border-right: 0;
	}

	.el-aside {
		color: white;
	}

	.aside {
		overflow: auto;
	}
	.top-content{
		display: flex;
		justify-content: space-between;
	}
</style>
