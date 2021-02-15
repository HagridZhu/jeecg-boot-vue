<template>
  <a-card :bordered="false">
    <a-form @submit="handleSubmit" :form="form">
      <a-row>
        <a-col :md="24" :sm="24">
          <a-form-item label="手机" :labelCol="{ span: 7 }" :wrapperCol="{ span: 15 }">
            <a-input v-decorator="['mobile',{rules: [{ required: true, message: '输入手机号，多个用英文逗号隔开' }]}]"/>
          </a-form-item>
        </a-col>
      </a-row>
      <a-row>
        <a-col :md="24" :sm="24">
          <a-form-item label="操作" :labelCol="{ span: 7 }" :wrapperCol="{ span: 15 }">
            <j-dict-select-tag placeholder="请选择状态" v-model="queryParam.logoStatus" dictCode="logo_status"/>
            <!-- <a-select v-decorator="['action',{rules: [{ required: true, message: '请选择执行的操作' }]}]" placeholder="选择执行的操作" @change="this.handleSelectChange">
              <a-select-option value="male">male</a-select-option>
              <a-select-option value="female">female</a-select-option>
            </a-select> -->
          </a-form-item>
        </a-col>
      </a-row>
      <a-row>
        <a-col :md="24" :sm="24">
          <a-form-item label="Gender" :labelCol="{ span: 7 }" :wrapperCol="{ span: 15 }">
            <a-cascader :options="areaOptions" @change="onChange" :showSearch="{filter}" placeholder="Please select" />
          </a-form-item>
        </a-col>
      </a-row>
      <a-form-item :wrapperCol="{ span: 12, offset: 5 }">
        <a-col :md="24" :sm="24">
          <a-form-item :wrapperCol="{ span: 12, offset: 5 }">
            <a-button type="primary" htmlType="submit">提交</a-button>
          </a-form-item>
        </a-col>
      </a-form-item>
    </a-form>
  </a-card>
</template>

<script>
  import { getAction } from '@/api/manage'
  export default {
    props: ['sex','name'],
    data () {
      return {
        submitUrl: '',
        formLayout: 'horizontal',
        form: this.$form.createForm(this),
        areaOptions:[]
      }
    },
    methods: {
      handleSubmit (e) {
        e.preventDefault()
        this.form.validateFields((err, values) => {
          if (!err) {
            console.log('Received values of form: ', values)
          }
        })
      },
      handleSelectChange (value) {
        console.log(value)
        this.form.setFieldsValue({
          note: `Hi, ${value === 'male' ? 'man' : 'lady'}!`,
        })
      },
      onChange(value, selectedOptions) {
        console.log(value, selectedOptions);
      },
      filter(inputValue, path) {
        return (path.some(option => (option.label).toLowerCase().indexOf(inputValue.toLowerCase()) > -1));
      },
    },
    created (){
      console.log('============= online href common props ============= ');
      console.log('props sex: ',this.sex);
      console.log('props name: ',this.name);

      getAction('/mock/api/area').then((res) => {
          console.log("------------")
          console.log(res)
          this.areaOptions = res;
      })
    },
    watch: {
      $route: {
        immediate: true,
        handler() {
          console.log('============= online href  $route props ============= ');
          let sex = this.$route.query.sex
          console.log('$route sex: ', sex);
        }
      }
    },
  }
</script>