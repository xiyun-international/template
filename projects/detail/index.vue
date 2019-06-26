<template>
    <div>
        <xy-title>详情</xy-title>
        <el-form ref="form" :model="form" label-width="120px">
            <el-form-item label="名称：" prop="name">
                <span>{{ form.name }}</span>
            </el-form-item>
            <el-form-item label="状态：" prop="status">
                <span>{{ form.status }}</span>
            </el-form-item>
            <el-form-item label="图片" prop="imageUrl">
                <img :src="form.imageUrl" alt="">
            </el-form-item>
        </el-form>
    </div>
</template>

<script>
export default {
    name: 'detail',
    data() {
        return {
            form: {},

            // 获取路由中的id值
            id: this.$route.params.id,
        }
    },
    created() {
        this.getDetailList()
    },
    methods: {
        getDetailList() {
            this.$post('/v1/detail', { id: this.id }).then(res => {
                if (res.code === 10000) {
                    this.form = res.bizContent.resultList;
                }
            })
        },
    },
}
</script>

