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
          <a-form-item label="销售品编码" :labelCol="{ span: 7 }" :wrapperCol="{ span: 15 }">
            <a-input  v-decorator="['saleProdNum',{rules: [{ required: true, message: '请输入销售品编码' }]}]"/>
          </a-form-item>
        </a-col>
      </a-row>
      <a-row>
        <a-col :md="24" :sm="24">
          <a-form-item label="销售品" :labelCol="{ span: 7 }" :wrapperCol="{ span: 15 }">
            <!-- <j-search-select-tag placeholder="请选择销售品" -->
            <j-dict-select-tag placeholder="请选择销售品" 
              :trigger-change="true"
              ref="saleProdSelect"
              type="search" 
              dictCode="sale_prod_type" @change="this.handleSelectChange"
              v-decorator="['saleProdSelect',{}]" />
            <!-- <a-select v-decorator="['action',{rules: [{ required: true, message: '请选择执行的操作' }]}]" placeholder="选择执行的操作" @change="this.handleSelectChange">
              <a-select-option value="male">male</a-select-option>
              <a-select-option value="female">female</a-select-option>
            </a-select> -->
          </a-form-item>
        </a-col>
      </a-row>

      <a-row v-show="false">
        <a-col :md="24" :sm="24" >
          <a-form-item label="销售品名称" :labelCol="{ span: 7 }" :wrapperCol="{ span: 15 }">
            <a-input  v-decorator="['saleProdName',{}]"/>
          </a-form-item>
        </a-col>
      </a-row>
      <a-row>
        <a-col :md="24" :sm="24">
          <a-form-item label="操作" :labelCol="{ span: 7 }" :wrapperCol="{ span: 15 }">
            <a-select v-decorator="['action',{rules: [{ required: true, message: '请选择执行的操作' }]}]" 
                       placeholder="选择执行的操作" >
              <a-select-option value="ADD">订购</a-select-option>
              <a-select-option value="DEL">退订</a-select-option>
            </a-select>
          </a-form-item>
        </a-col>
      </a-row>
      <!-- <a-row>
        <a-col :md="24" :sm="24">
          <a-form-item label="Gender" :labelCol="{ span: 7 }" :wrapperCol="{ span: 15 }">
            <a-cascader :options="areaOptions" @change="onChange" :showSearch="{filter}" placeholder="Please select" />
          </a-form-item>
        </a-col>
      </a-row> -->
      <a-form-item :wrapperCol="{ span: 12, offset: 5 }">
        <a-col :md="24" :sm="24">
          <a-form-item :wrapperCol="{ span: 12, offset: 5 }">
            <a-button type="primary" htmlType="submit" :loading="loading" >提交</a-button>
          </a-form-item>
        </a-col>
      </a-form-item>
    </a-form>

    <!-- 返回结果 -->
    <a-tabs type="card" default-active-key="1">
      <a-tab-pane key="1" tab="返回">
        <xmp><div v-text="result"></div></xmp>
      </a-tab-pane>
    </a-tabs>

  </a-card>
</template>

<script>
  import { postAction } from '@/api/manage'
  import vkbeautify from 'vkbeautify'
  export default {
    props: ['sex','name'],
    data () {
      return {
        submitUrl: '/order/open/vip/api',
        loading: false,
        param: {},
        formLayout: 'horizontal',
        form: this.$form.createForm(this),
        areaOptions:[],
        result: ''
      }
    },
    methods: {
      handleSubmit (e) {
        e.preventDefault()
        this.form.validateFields((err, values) => {
          if (!err) {
            console.log('Received values of form: ', values)
            let formData = values;
            delete formData.saleProdSelect

            //提交数据
            this.loading = true
            this.result = ''
            postAction(this.submitUrl, formData).then(res => {
              this.result = vkbeautify.xml(res.result || res.message)
              // 手机号置空
              this.form.setFieldsValue({
                mobile: ''
              })
              this.loading = false
            })
          }
        })
      },
      handleSelectChange (value) {
        // 获取选中的字典
        var dict = this.$refs.saleProdSelect.dictOptions.find(function(item){
          return item.value === value
        })
        this.form.setFieldsValue({
          saleProdName: dict.text,
          saleProdNum: dict.value
        })

      },
      // onChange(value, selectedOptions) {
      //   console.log(value, selectedOptions);
      // },
      filter(inputValue, path) {
        return (path.some(option => (option.label).toLowerCase().indexOf(inputValue.toLowerCase()) > -1));
      },
    },
    created (){
      console.log('============= online href common props ============= ');
      console.log('props sex: ',this.sex);
      console.log('props name: ',this.name);

      // getAction('/mock/api/area').then((res) => {
      //     console.log("------------")
      //     console.log(res)
      //     this.areaOptions = res;
      // })
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