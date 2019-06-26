<template>
    <div>
        <xy-title>创建</xy-title>
        <xy-wrapper>
            <el-form ref="form" :model="form" label-width="120px">
                <el-form-item label="名称：" prop="name">
                    <el-input placeholder="请输入名称" v-model="form.name"></el-input>
                </el-form-item>
                <el-form-item label="状态：" prop="status">
                    <el-select placeholder="请选择状态" v-model="form.status" style="width: 100%">
                        <el-option value="">请选择</el-option>
                    </el-select>
                </el-form-item>
                <el-form-item label="性别：" prop="age">
                    <el-radio-group v-model="form.age">
                        <el-radio :label="1">男</el-radio>
                        <el-radio :label="2">女</el-radio>
                    </el-radio-group>
                </el-form-item>

                <el-row>
                    <el-col :span="16">
                        <el-form-item>
                            <el-button type="primary" @click="onSubmit">
                                提交
                            </el-button>
                            <el-button type="default" @click="resetForm">
                                取消
                            </el-button>
                        </el-form-item>
                    </el-col>
                </el-row>
            </el-form>
        </xy-wrapper>
    </div>
</template>

<script>
export default {
    name: 'template-create',
    data() {
        return {
            form: {},
        }
    },
    methods: {
        /**
         *  点击提交
         */
        onSubmit() {
            const params = this.form
            this.$post('/v1/save', params).then(res => {
                if (res.code === 200) {
                    this.$message({
                        type: 'success',
                        message: res.message,
                    })
                }
            })
        },

        /**
         *  点击取消
         */
        resetForm() {
            this.$confirm('确定取消创建？', '提示', {
                confirmButtonText: '确定',
                cancelButtonText: '取消',
                type: 'warning',
            })
        },
    },
}
</script>

