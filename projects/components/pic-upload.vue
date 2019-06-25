<template>
    <el-upload
        class="avatar-uploader"
        :action="action"
        :headers="{ Authorization: token }"
        :show-file-list="false"
        :on-success="handleUploadSuccess"
        :before-upload="beforeUpload"
        name="attachmentImage"
    >
        <img v-if="imageUrl" :src="imageUrl" class="avatar" alt="" />
        <i v-loading="loading" v-else class="el-icon-plus avatar-uploader-icon"></i>
    </el-upload>
</template>

<script>
export default {
    name: 'picUpload',
    data() {
        return {
            imageUrl: '',
            token: '',
            loading: false,
        }
    },
    props: {
        action: {
            type: String,
            default() {
                return '/v1/imageUpload';
            },
        },
    },
    methods: {
        /**
         *  图片上传成功时
         */
        handleUploadSuccess(res) {
            if (res && res.code === 200) {
                // 预览图
                this.imageUrl = res.bizContent.attachmentThumbUrl;
                this.$message.success('上传成功');
                this.$emit('input', res.bizContent.attachmentUrl);
            } else {
                this.$message.error((res && res.message) || '上传图片失败');
            }
            this.loading = false;
        },

        /**
         *  对图片进行格式限制
         */
        checkImage(file) {
            const isValid = false;
            const isImage =
                file.type === 'image/jpeg' || file.type === 'image/bmp' || file.type === 'image/png';
            if (!isImage) {
                this.$message.error('只能上传 jpg、jpeg、bmp、png 类型的图片!');
                return isValid;
            }

            const isLt5M = file.size / 1024 / 1024 < 1;
            if (!isLt5M) {
                this.$message.error('图片大小不能超过 1MB!');
                return isValid;
            }

            return !isValid;
        },

        /**
         *  上传图片之前进行格式限制
         */
        beforeUpload(file) {
            this.loading = true;
            const isValid = this.checkImage(file);
            if (!isValid) {
                this.loading = false;
            } else {
                this.imageUrl = '';
            }
            return isValid;
        },
    },
}
</script>

<style  scoped>
    .avatar-uploader /deep/ .el-upload {
        border: 1px dashed #d9d9d9;
        border-radius: 6px;
        cursor: pointer;
        position: relative;
        overflow: hidden;
    }
    .avatar-uploader /deep/ .el-upload:hover {
        border-color: #409EFF;
    }
    .avatar-uploader-icon {
        font-size: 28px;
        color: #8c939d;
        width: 178px;
        height: 178px;
        line-height: 178px;
        text-align: center;
    }
    .avatar {
        width: 178px;
        height: 178px;
        display: block;
    }
</style>

