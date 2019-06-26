<template>
    <div>
        <xy-title>列表</xy-title>
        <xy-wrapper>
            <search-form @onSearch="onSearch"></search-form>
        </xy-wrapper>
        <div class="t-MT28">
            <router-link to="/create">
                <el-button type="primary">创建</el-button>
            </router-link>
            <el-button type="default">导出 Excal</el-button>
        </div>
        <xy-wrapper class="t-MT28">
            <el-table ref="table" :data="tableData" style="width: 100%">
                <el-table-column label="姓名">
                    <template>
                        禧云001
                    </template>
                </el-table-column>
                <el-table-column label="状态">
                    <template>
                        状态1
                    </template>
                </el-table-column>
                <el-table-column label="操作" align="center" min-width="80">
                    <template slot-scope="scope">
                        <router-link :to="`/detail/${scope.row.userId}`">
                            <el-button size="small" type="text">查看</el-button>
                        </router-link>
                        <router-link :to="`/edit/${scope.row.userId}`">
                            <el-button size="small" type="text">编辑</el-button>
                        </router-link>
                        <router-link :to="`/change/${scope.row.userId}`">
                            <el-button size="small" type="text">变更</el-button>
                        </router-link>
                    </template>
                </el-table-column>
            </el-table>
        </xy-wrapper>

        <div class="t-MT28">
            <el-pagination
              @current-change="handleCurrentChange"
              @size-change="handleSizeChange"
              :current-page="page.currentPage"
              :page-size="page.pageSize"
              :page-sizes="[10, 20, 50, 100]"
              layout="total, sizes, prev, pager, next, jumper"
              :total="page.total"
            >
            </el-pagination>
        </div>
    </div>
</template>

<script>
import SearchForm from './form/form.vue';

export default {
    name: 'list',
    data() {
        return {
            searchForm: {},

            // 表格数据
            tableData: [],

            // 页脚
            page: {
                currentPage: 1,
                total: 0,
                pageSize: 10,
            },
        }
    },
    components: {
        SearchForm,
    },
    created() {
        this.getListData();
    },
    methods: {
        /**
         *  获取列表页数据
         */
        getListData() {
            this.$post('/v1/list', params).then(res => {
                this.tableData = res.bizContent.resultList;
            })
        },

        /**
         *  点击搜索按钮
         */
        onSearch(form) {
            this.searchForm = form;
            this.getListData({
                ...this.searchForm,
                currentPage: this.page.currentPage,
                pageSize: this.page.pageSize,
            })
        },

        /**
         *  设置分页大小
         */
        handleSizeChange(val) {
            this.page.pageSize = val;
            this.getListData({
                ...this.searchForm,
                currentPage: this.page.currentPage,
                pageSize: this.page.pageSize,
            })
        },

        /**
         *  指定当前到第几页
         */
        handleCurrentChange(val) {
            this.page.currentPage = val;
            this.getListData({
                ...this.searchForm,
                currentPage: this.page.currentPage,
                pageSize: this.page.pageSize,
            })
        },
    },
}
</script>

