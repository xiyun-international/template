<template>
    <div>
        <xy-title>XXX列表</xy-title>
        <xy-wrapper> <search-form @onSearch="onSearch"></search-form> </xy-wrapper>
        <div class="mt-24">
            <el-button type="primary" @click="goCreate">创建</el-button>
            <el-button type="default">导出 Excal</el-button>
        </div>
        <xy-wrapper class="mt-24">
            <el-table ref="table" :data="tableData" style="width: 100%">
                <el-table-column label="编号">
                    <template slot-scope="scope">
                        <div> {{ scope.row.code }} </div>
                    </template>
                </el-table-column>
                <el-table-column label="姓名">
                    <template slot-scope="scope">
                        <div> {{ scope.row.name }} </div>
                    </template>
                </el-table-column>
                <el-table-column label="分类">
                    <template slot-scope="scope">
                        <div> {{ scope.row.type }} </div>
                    </template>
                </el-table-column>
                <el-table-column label="状态">
                    <template slot-scope="scope">
                        <div> {{ scope.row.status }} </div>
                    </template>
                </el-table-column>
                <el-table-column label="手机号码">
                    <template slot-scope="scope">
                        <div> {{ scope.row.tel }} </div>
                    </template>
                </el-table-column>
                <el-table-column label="操作" align="center" min-width="80">
                    <template slot-scope="scope">
                        <el-button
                            @click="goDetail(scope.row.userId)"
                            type="text"
                            size="small"
                        >
                            查看
                        </el-button>
                        <el-button 
                            @click="goEdit(scope.row.userId)" 
                            type="text" 
                            size="small"
                        >
                            编辑
                        </el-button>
                        <el-button 
                            type="text" 
                            size="small" 
                            @click="goChange(scope.row.userId)"
                        >
                            变更
                        </el-button>
                    </template>
                </el-table-column>
            </el-table>
        </xy-wrapper>

        <div class="mt-28" style="text-align: right">
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
            tableData: [
                {
                   userId: 1,
                   code: '1111111',
                   name: '禧云',
                   type: '测试001',
                   status: '状态1',
                   tel: '18588888888', 
                },
                {
                   userId: 2,
                   code: '222222',
                   name: '禧云',
                   type: '测试002',
                   status: '状态2',
                   tel: '18566666666', 
                },
            ],

            // 页脚
            page: {
                currentPage: 1,
                total: 0,
                pageSize: 10,
            },

            // 获取路由信息
            path: this.$route.path,
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
         *  点击跳转到创建页面
         */
        goCreate() {
            this.$router.push(`${this.path}/create`);
        },

        /**
         *  点击跳转详情页面
         */
        goDetail(id) {
            this.$router.push(`${this.path}/detail/${id}`);
        },

        /**
         *  点击跳转编辑页面
         */
        goEdit(id) {
            this.$router.push(`${this.path}/edit/${id}`);
        },

        /**
         *  点击跳转变更页面
         */
        goChange(id) {
            this.$router.push(`${this.path}/change/${id}`);
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

