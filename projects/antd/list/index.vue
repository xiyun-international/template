<template>
  <xy-context
    :breadcrumb="[{ name: '列表', path: 'biz/list' }, { name: '用户列表', path: 'biz/list' }]"
    title="用户列表"
    tag="审核中"
    :tag-status="5"
  >
    <div slot="right">
      <a-button type="primary">打印</a-button>
    </div>

    <a-row>
      <a-col>
        <search-form @onSearch="onSearch"></search-form>
      </a-col>
    </a-row>

    <a-row class="t-MT30">
      <a-table
        :rowKey="record => record.id"
        :columns="columns"
        :dataSource="List"
        :pagination="true"
      >
        <template slot="action">
          <router-link to="/biz/detail">
            查看
          </router-link>
        </template>
      </a-table>
    </a-row>
  </xy-context>
</template>

<script>
import SearchForm from './form/search.vue';

export default {
  name: 'listDemo',
  data() {
    return {
      searchForm: {},
      pagination: {
        page: 1,
        pageSize: 10,
        totalCount: 0,
      },
      columns: [
        {
          title: '单号',
          dataIndex: 'code',
          width: '30%',
        },
        {
          title: '时间',
          dataIndex: 'time',
        },
        {
          title: '状态',
          dataIndex: 'status',
        },
        {
          title: '操作',
          dataIndex: 'action',
          scopedSlots: { customRender: 'action' },
        },
      ],
      List: [
        {
          id: 1,
          code: 1111,
          time: '2019.7.30',
          status: '没状态',
        },
      ],
    };
  },
  components: {
    SearchForm,
  },
  methods: {
    /**
     *  获取列表页数据
     */
    getList(params = {}) {
      this.$post('/list', params).then(() => {
        this.$message({
          type: 'success',
        });
      });
    },

    onSearch(form) {
      this.searchForm = form;
      this.getList({
        ...this.searchForm,
        page: 1,
        pageSize: 10,
      });
    },
  },
};
</script>

<style scoped></style>
