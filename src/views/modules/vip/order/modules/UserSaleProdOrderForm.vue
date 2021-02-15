<template>
  <a-spin :spinning="confirmLoading">
    <j-form-container :disabled="formDisabled">
      <a-form :form="form" slot="detail">
        <a-row>
          <a-col :span="24">
            <a-form-item label="订单id" :labelCol="labelCol" :wrapperCol="wrapperCol">
              <a-input-number v-decorator="['saleProdOrderId', validatorRules.saleProdOrderId]" placeholder="请输入订单id" style="width: 100%" />
            </a-form-item>
          </a-col>
          <a-col :span="24">
            <a-form-item label="用户id" :labelCol="labelCol" :wrapperCol="wrapperCol">
              <a-input-number v-decorator="['userId']" placeholder="请输入用户id" style="width: 100%" />
            </a-form-item>
          </a-col>
          <a-col :span="24">
            <a-form-item label="销售品id" :labelCol="labelCol" :wrapperCol="wrapperCol">
              <a-input v-decorator="['saleProdId']" placeholder="请输入销售品id"  ></a-input>
            </a-form-item>
          </a-col>
          <a-col :span="24">
            <a-form-item label="合同id" :labelCol="labelCol" :wrapperCol="wrapperCol">
              <a-input-number v-decorator="['contractId']" placeholder="请输入合同id" style="width: 100%" />
            </a-form-item>
          </a-col>
          <a-col :span="24">
            <a-form-item label="订单状态" :labelCol="labelCol" :wrapperCol="wrapperCol">
              <j-multi-select-tag type="list_multi" v-decorator="['orderStatus']" :trigger-change="true" dictCode="order_status" placeholder="请选择订单状态" />
            </a-form-item>
          </a-col>
          <a-col :span="24">
            <a-form-item label="开始时间" :labelCol="labelCol" :wrapperCol="wrapperCol">
              <j-date placeholder="请选择开始时间" v-decorator="['beginTime']" :trigger-change="true" :show-time="true" date-format="YYYY-MM-DD HH:mm:ss" style="width: 100%" />
            </a-form-item>
          </a-col>
          <a-col :span="24">
            <a-form-item label="结束时间" :labelCol="labelCol" :wrapperCol="wrapperCol">
              <j-date placeholder="请选择结束时间" v-decorator="['endTime']" :trigger-change="true" :show-time="true" date-format="YYYY-MM-DD HH:mm:ss" style="width: 100%" />
            </a-form-item>
          </a-col>
          <a-col :span="24">
            <a-form-item label="创建时间" :labelCol="labelCol" :wrapperCol="wrapperCol">
              <j-date placeholder="请选择创建时间" v-decorator="['createDate']" :trigger-change="true" :show-time="true" date-format="YYYY-MM-DD HH:mm:ss" style="width: 100%" />
            </a-form-item>
          </a-col>
          <a-col :span="24">
            <a-form-item label="更新时间" :labelCol="labelCol" :wrapperCol="wrapperCol">
              <j-date placeholder="请选择更新时间" v-decorator="['modifyDate']" :trigger-change="true" :show-time="true" date-format="YYYY-MM-DD HH:mm:ss" style="width: 100%" />
            </a-form-item>
          </a-col>
          <a-col :span="24">
            <a-form-item label="支付时间" :labelCol="labelCol" :wrapperCol="wrapperCol">
              <j-date placeholder="请选择支付时间" v-decorator="['payTime']" :trigger-change="true" :show-time="true" date-format="YYYY-MM-DD HH:mm:ss" style="width: 100%" />
            </a-form-item>
          </a-col>
          <a-col :span="24">
            <a-form-item label="产品类型" :labelCol="labelCol" :wrapperCol="wrapperCol">
              <j-dict-select-tag type="list" v-decorator="['productType']" :trigger-change="true" dictCode="product_type" placeholder="请选择产品类型" />
            </a-form-item>
          </a-col>
          <a-col :span="24">
            <a-form-item label="计费单元" :labelCol="labelCol" :wrapperCol="wrapperCol">
              <j-dict-select-tag type="list" v-decorator="['currencyUnit']" :trigger-change="true" dictCode="currency_unit" placeholder="请选择计费单元" />
            </a-form-item>
          </a-col>
          <a-col :span="24">
            <a-form-item label="金额" :labelCol="labelCol" :wrapperCol="wrapperCol">
              <a-input-number v-decorator="['chargeMoney']" placeholder="请输入金额" style="width: 100%" />
            </a-form-item>
          </a-col>
          <a-col :span="24">
            <a-form-item label="真实支付金额" :labelCol="labelCol" :wrapperCol="wrapperCol">
              <a-input-number v-decorator="['realPayMoney']" placeholder="请输入真实支付金额" style="width: 100%" />
            </a-form-item>
          </a-col>
          <a-col :span="24">
            <a-form-item label="支付类型" :labelCol="labelCol" :wrapperCol="wrapperCol">
              <j-dict-select-tag type="list" v-decorator="['payType']" :trigger-change="true" dictCode="pay_type" placeholder="请选择支付类型" />
            </a-form-item>
          </a-col>
          <a-col :span="24">
            <a-form-item label="支付渠道" :labelCol="labelCol" :wrapperCol="wrapperCol">
              <a-input v-decorator="['payChannel']" placeholder="请输入支付渠道"  ></a-input>
            </a-form-item>
          </a-col>
          <a-col :span="24">
            <a-form-item label="支付者ID" :labelCol="labelCol" :wrapperCol="wrapperCol">
              <a-input v-decorator="['payId']" placeholder="请输入支付者ID"  ></a-input>
            </a-form-item>
          </a-col>
          <a-col :span="24">
            <a-form-item label="支付者IP" :labelCol="labelCol" :wrapperCol="wrapperCol">
              <a-input v-decorator="['payUserIpaddr']" placeholder="请输入支付者IP"  ></a-input>
            </a-form-item>
          </a-col>
          <a-col :span="24">
            <a-form-item label="是否有拓展值" :labelCol="labelCol" :wrapperCol="wrapperCol">
              <j-dict-select-tag type="list" v-decorator="['hasAttr']" :trigger-change="true" dictCode="yes_or_no" placeholder="请选择是否有拓展值" />
            </a-form-item>
          </a-col>
          <a-col :span="24">
            <a-form-item label="appId" :labelCol="labelCol" :wrapperCol="wrapperCol">
              <a-input-number v-decorator="['appId']" placeholder="请输入appId" style="width: 100%" />
            </a-form-item>
          </a-col>
          <a-col :span="24">
            <a-form-item label="备注" :labelCol="labelCol" :wrapperCol="wrapperCol">
              <a-input v-decorator="['remarks']" placeholder="请输入备注"  ></a-input>
            </a-form-item>
          </a-col>
          <a-col :span="24">
            <a-form-item label="nmsc_streaming_no" :labelCol="labelCol" :wrapperCol="wrapperCol">
              <a-input v-decorator="['nmscStreamingNo']" placeholder="请输入nmsc_streaming_no"  ></a-input>
            </a-form-item>
          </a-col>
          <a-col :span="24">
            <a-form-item label="nmsc_product_id" :labelCol="labelCol" :wrapperCol="wrapperCol">
              <a-input v-decorator="['nmscProductId']" placeholder="请输入nmsc_product_id"  ></a-input>
            </a-form-item>
          </a-col>
          <a-col :span="24">
            <a-form-item label="是否新用户" :labelCol="labelCol" :wrapperCol="wrapperCol">
              <j-dict-select-tag type="list" v-decorator="['isNewCustomer']" :trigger-change="true" dictCode="yes_or_no" placeholder="请选择是否新用户" />
            </a-form-item>
          </a-col>
          <a-col :span="24">
            <a-form-item label="关联订单id" :labelCol="labelCol" :wrapperCol="wrapperCol">
              <a-input-number v-decorator="['relOrderid']" placeholder="请输入关联订单id" style="width: 100%" />
            </a-form-item>
          </a-col>
          <a-col :span="24">
            <a-form-item label="是否开发票" :labelCol="labelCol" :wrapperCol="wrapperCol">
              <a-input-number v-decorator="['issueinvoice']" placeholder="请输入是否开发票" style="width: 100%" />
            </a-form-item>
          </a-col>
          <a-col :span="24">
            <a-form-item label="iap_receipt_verify_resp" :labelCol="labelCol" :wrapperCol="wrapperCol">
              <a-input v-decorator="['iapReceiptVerifyResp']" placeholder="请输入iap_receipt_verify_resp"  ></a-input>
            </a-form-item>
          </a-col>
          <a-col :span="24">
            <a-form-item label="iap_transaction_receipt" :labelCol="labelCol" :wrapperCol="wrapperCol">
              <a-input v-decorator="['iapTransactionReceipt']" placeholder="请输入iap_transaction_receipt"  ></a-input>
            </a-form-item>
          </a-col>
          <a-col :span="24">
            <a-form-item label="拓展字段1" :labelCol="labelCol" :wrapperCol="wrapperCol">
              <a-input v-decorator="['extFields1']" placeholder="请输入拓展字段1"  ></a-input>
            </a-form-item>
          </a-col>
          <a-col :span="24">
            <a-form-item label="拓展字段2" :labelCol="labelCol" :wrapperCol="wrapperCol">
              <a-input v-decorator="['extFields2']" placeholder="请输入拓展字段2"  ></a-input>
            </a-form-item>
          </a-col>
          <a-col v-if="showFlowSubmitButton" :span="24" style="text-align: center">
            <a-button @click="submitForm">提 交</a-button>
          </a-col>
        </a-row>
      </a-form>
    </j-form-container>
  </a-spin>
</template>

<script>

  import { httpAction, getAction } from '@/api/manage'
  import pick from 'lodash.pick'
  import { validateDuplicateValue } from '@/utils/util'

  export default {
    name: 'UserSaleProdOrderForm',
    components: {
    },
    props: {
      //流程表单data
      formData: {
        type: Object,
        default: ()=>{},
        required: false
      },
      //表单模式：true流程表单 false普通表单
      formBpm: {
        type: Boolean,
        default: false,
        required: false
      },
      //表单禁用
      disabled: {
        type: Boolean,
        default: false,
        required: false
      }
    },
    data () {
      return {
        form: this.$form.createForm(this),
        model: {},
        labelCol: {
          xs: { span: 24 },
          sm: { span: 5 },
        },
        wrapperCol: {
          xs: { span: 24 },
          sm: { span: 16 },
        },
        confirmLoading: false,
        validatorRules: {
          saleProdOrderId: {
            rules: [
              { required: true, message: '请输入订单id!'},
            ]
          },
        },
        url: {
          add: "/order/userSaleProdOrder/add",
          edit: "/order/userSaleProdOrder/edit",
          queryById: "/order/userSaleProdOrder/queryById"
        }
      }
    },
    computed: {
      formDisabled(){
        if(this.formBpm===true){
          if(this.formData.disabled===false){
            return false
          }
          return true
        }
        return this.disabled
      },
      showFlowSubmitButton(){
        if(this.formBpm===true){
          if(this.formData.disabled===false){
            return true
          }
        }
        return false
      }
    },
    created () {
      //如果是流程中表单，则需要加载流程表单data
      this.showFlowData();
    },
    methods: {
      add () {
        this.edit({});
      },
      edit (record) {
        this.form.resetFields();
        this.model = Object.assign({}, record);
        this.visible = true;
        this.$nextTick(() => {
          this.form.setFieldsValue(pick(this.model,'saleProdOrderId','userId','saleProdId','contractId','orderStatus','beginTime','endTime','createDate','modifyDate','payTime','productType','currencyUnit','chargeMoney','realPayMoney','payType','payChannel','payId','payUserIpaddr','hasAttr','appId','remarks','nmscStreamingNo','nmscProductId','isNewCustomer','relOrderid','issueinvoice','iapReceiptVerifyResp','iapTransactionReceipt','extFields1','extFields2'))
        })
      },
      //渲染流程表单数据
      showFlowData(){
        if(this.formBpm === true){
          let params = {id:this.formData.dataId};
          getAction(this.url.queryById,params).then((res)=>{
            if(res.success){
              this.edit (res.result);
            }
          });
        }
      },
      submitForm () {
        const that = this;
        // 触发表单验证
        this.form.validateFields((err, values) => {
          if (!err) {
            that.confirmLoading = true;
            let httpurl = '';
            let method = '';
            if(!this.model.id){
              httpurl+=this.url.add;
              method = 'post';
            }else{
              httpurl+=this.url.edit;
               method = 'put';
            }
            let formData = Object.assign(this.model, values);
            console.log("表单提交数据",formData)
            httpAction(httpurl,formData,method).then((res)=>{
              if(res.success){
                that.$message.success(res.message);
                that.$emit('ok');
              }else{
                that.$message.warning(res.message);
              }
            }).finally(() => {
              that.confirmLoading = false;
            })
          }
         
        })
      },
      popupCallback(row){
        this.form.setFieldsValue(pick(row,'saleProdOrderId','userId','saleProdId','contractId','orderStatus','beginTime','endTime','createDate','modifyDate','payTime','productType','currencyUnit','chargeMoney','realPayMoney','payType','payChannel','payId','payUserIpaddr','hasAttr','appId','remarks','nmscStreamingNo','nmscProductId','isNewCustomer','relOrderid','issueinvoice','iapReceiptVerifyResp','iapTransactionReceipt','extFields1','extFields2'))
      },
    }
  }
</script>