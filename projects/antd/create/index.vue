<template>
  <xy-context :breadcrumb="[{ name: '用户管理', path: '' }, { name: '新增用户' }]" title="新增用户">
    <xy-title style="margin-top: unset;">基础信息</xy-title>
    <a-form :form="form" @submit="onSubmit">
      <a-form-item v-bind="middleLayout" label="用户名">
        <a-input
          v-decorator="[
            'username',
            {
              rules: [{ required: true, message: '请填写用户名' }],
            },
          ]"
          placeholder="请填写用户名"
          :maxLength="20"
        />
      </a-form-item>
      <a-form-item v-bind="middleLayout" label="E-mail">
        <a-input
          v-decorator="[
            'email',
            {
              rules: [
                {
                  type: 'email',
                  message: 'E-mail 不对',
                },
                {
                  required: true,
                  message: '请输入 E-mail!',
                },
              ],
            },
          ]"
          placeholder="请填写 E-mail"
        />
      </a-form-item>
      <a-form-item v-bind="middleLayout" label="密码">
        <a-input
          v-decorator="[
            'password',
            {
              rules: [
                {
                  required: true,
                  message: '请填写密码',
                },
              ],
            },
          ]"
          type="password"
          placeholder="请填写密码"
        />
      </a-form-item>
      <a-form-item v-bind="middleLayout" label="性别">
        <a-radio-group
          v-decorator="[
            'gender',
            {
              rules: [
                {
                  required: true,
                },
              ],
              initialValue: 1,
            },
          ]"
        >
          <a-radio :value="1">男</a-radio>
          <a-radio :value="2">女</a-radio>
        </a-radio-group>
      </a-form-item>
      <a-form-item v-bind="buttonLayout">
        <a-button type="primary" htmlType="submit">
          提交审核
        </a-button>
      </a-form-item>
    </a-form>
  </xy-context>
</template>

<script>
export default {
  name: 'createDemo',
  data() {
    return {
      form: this.$form.createForm(this),
      middleLayout: {
        labelCol: { span: 8 },
        wrapperCol: { span: 8 },
      },
      buttonLayout: {
        wrapperCol: { span: 8, push: 8 },
      },
    };
  },
  methods: {
    onSubmit(e) {
      e.preventDefault();
      this.form.validateFields((err, values) => {
        if (err) {
          return;
        }
        this.$post('/create', values).then(() => {
          this.$message.success('创建成功');
          this.$router.replace('/biz/list');
        });
      });
    },
  },
};
</script>

<style scoped></style>
