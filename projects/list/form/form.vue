<template>
    <el-form ref="form" :model="form" label-width="120px">
        <el-row>
            <el-col :span="12">
                <el-form-item label="团餐客户" prop="supplierName">
                    <el-input placeholder="请输入团餐客户名称" v-model="form.supplierName"></el-input>
                </el-form-item>
            </el-col>
            <el-col :span="12">
                <el-form-item label="餐饮中心" prop="caterName">
                    <el-input placeholder="请输入餐饮中心名称" v-model="form.caterName"></el-input>
                </el-form-item>
            </el-col>
        </el-row>
        <el-row>
            <el-col :span="12">
                <el-form-item label="状态" prop="status">
                    <el-select placeholder="请选择状态" style="width: 100%" v-model="form.status">
                        <el-option value="请选择"></el-option>
                    </el-select>
                </el-form-item>
            </el-col>
            <el-col :span="12">
                <el-form-item label="支付通道" prop="payChannel">
                    <el-select placeholder="请选择支付通道" style="width: 100%" v-model="form.payChannel">
                        <el-option value="请选择"></el-option>
                    </el-select>
                </el-form-item>
            </el-col>
        </el-row>
        <el-row>
            <el-col :span="12">
                <el-form-item label="门店名称" prop="canteenName">
                    <el-input placeholder="请输入门店名称" v-model="form.canteenName"></el-input>
                </el-form-item>
            </el-col>
            <el-col :span="12">
                <el-form-item label="门店类型" prop="canteenType">
                    <el-select placeholder="请选择门店类型" style="width: 100%" v-model="form.canteenType">
                        <el-option value="请选择"></el-option>
                    </el-select>
                </el-form-item>
            </el-col>
        </el-row>

        <el-row>
            <el-col :span="24">
                <div style="text-align: right">
                    <el-button type="primary" @click="onSearch">查询</el-button>
                    <el-button type="default" @click="onReset">重置</el-button>
                </div>
            </el-col>
        </el-row>
    </el-form>
</template>

<script>
const defaultForm = {
    suplierName: '',
    caterName: '',
    status: '',
    payChannel: '',
    canteenName: '',
    canteenType: '',
}

export default {
    name: 'search-form',
    data() {
        return {
            form: { ...defaultForm },
        }
    },
    methods: {
        /**
         *  点击查询按钮
         */
        onSearch() {
            this.$emit('onSearch', this.form);
        },

        /**
         *  点击重置按钮
         */
        onReset() {
            this.$refs.form.resetFields();
        },
    },
}
</script>

