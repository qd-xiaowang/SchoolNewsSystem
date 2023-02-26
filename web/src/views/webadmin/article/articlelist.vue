<template>
  <div class="createhelplist">
    <div class="title">
      <div >
        <el-button
          type="text"
          @click="$router.push('/admin/createarticle') "
        >发布新闻</el-button>
      </div>
    </div>
    <el-table :data="tableData" border style="width: 100%">
		<el-table-column prop="article_title" label="新闻标题"></el-table-column>
		<el-table-column prop="article_lable" label="新闻分类"></el-table-column>
      <el-table-column fixed prop="createtime" label="日期">
        <template slot-scope="scope">{{ scope.row.article_createtime | dataFormat }}</template>
      </el-table-column>
      <el-table-column fixed="right" label="操作" width="100">
        <template slot-scope="scope">
          <el-button
            @click="$router.push(`/admin/updataarticle/${scope.row.article_id}`) "
            type="text"
            size="small"
          >编辑</el-button>
          <el-button type="text" size="small" @click="del(scope.row.article_id)">删除</el-button>
        </template>
      </el-table-column>
    </el-table>
    <el-pagination class='el-pagination1'
      @size-change="handleSizeChange"
      @current-change="handleCurrentChange"
      :page-sizes="[10, 20, 50, 100]"
      :page-size="pagelistquery.pagesize"
      layout="total, sizes, prev, pager, next, jumper"
      :total="pagelistquery.total"
    ></el-pagination>
  </div>
</template>

<script>
export default {
  data() {
    return {
      lable:[],
      pagelistquery: {
        total: 0,
        page: 1,
        pagesize: 10
      },
      tableData: []
    };
  },
  filters: {
    statefilter(state) {
      if (state == 1) {
        return "审核通过";
      }
      if (state == 0) {
        return "正在审核";
      }
      if (state == -1) {
        return "审核未通过";
      }
    }
  },
  methods: {
    async del(help_id) {
      let res = await this.$axios.post(
        "/webadmin/deletehelp",
        this.qs.stringify({ help_id: help_id })
      );
      if (res.data.state.type === "SUCCESS") {
        this.$message.success("删除成功");
        this.articlelist();
      }
    },
    handleSizeChange(val) {
      this.pagelistquery.pagesize = val;
      this.articlelist();
    },
    handleCurrentChange(val) {
      this.pagelistquery.page = val;
      this.articlelist();
    },
    async articlelist() {
      let data = {
        page: this.pagelistquery.page,
        pagesize: this.pagelistquery.pagesize
      };
      let res = await this.$axios.post(
        "/webadmin/articlelist",
        this.qs.stringify(data)
      );
      if (res.data.state.type === "SUCCESS") {
        this.tableData = res.data.data;
        this.pagelistquery.total = res.data.count;
      }
    },
  
  },
  created() {
    this.articlelist();
  }
};
</script>
<style  scoped>
	.title{
		display: flex;
		justify-content: flex-end;
	}
	::v-deep .cell{
		font-weight:400;
	}
</style>