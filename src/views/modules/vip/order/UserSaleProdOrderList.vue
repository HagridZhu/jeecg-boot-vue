<template>
  <a-card :bordered="false">
    <!-- 查询区域 -->
    <div class="table-page-search-wrapper">
      <a-form layout="inline" @keyup.enter.native="searchQuery">
        <a-row :gutter="24">
          <a-col :xl="6" :lg="7" :md="8" :sm="24">
            <a-form-item label="用户id">
              <a-input placeholder="请输入用户id" v-model="queryParam.userId"></a-input>
            </a-form-item>
          </a-col>
          <a-col :xl="6" :lg="7" :md="8" :sm="24">
            <a-form-item label="销售品id">
              <a-input placeholder="请输入销售品id" v-model="queryParam.saleProdId"></a-input>
            </a-form-item>
          </a-col>
          <template v-if="toggleSearchStatus">
            <a-col :xl="6" :lg="7" :md="8" :sm="24">
              <a-form-item label="合同id">
                <a-input placeholder="请输入合同id" v-model="queryParam.contractId"></a-input>
              </a-form-item>
            </a-col>
            <a-col :xl="6" :lg="7" :md="8" :sm="24">
              <a-form-item label="订单状态">
                <j-multi-select-tag placeholder="请选择订单状态" dictCode="order_status" v-model="queryParam.orderStatus_MultiString"/>
              </a-form-item>
            </a-col>
            <a-col :xl="6" :lg="7" :md="8" :sm="24">
              <a-form-item label="产品类型">
                <j-dict-select-tag placeholder="请选择产品类型" v-model="queryParam.productType" dictCode="product_type"/>
              </a-form-item>
            </a-col>
            <a-col :xl="6" :lg="7" :md="8" :sm="24">
              <a-form-item label="计费单元">
                <j-dict-select-tag placeholder="请选择计费单元" v-model="queryParam.currencyUnit" dictCode="currency_unit"/>
              </a-form-item>
            </a-col>
            <a-col :xl="6" :lg="7" :md="8" :sm="24">
              <a-form-item label="支付类型">
                <j-dict-select-tag placeholder="请选择支付类型" v-model="queryParam.payType" dictCode="pay_type"/>
              </a-form-item>
            </a-col>
          </template>
          <a-col :xl="6" :lg="7" :md="8" :sm="24">
            <span style="float: left;overflow: hidden;" class="table-page-search-submitButtons">
              <a-button type="primary" @click="searchQuery" icon="search">查询</a-button>
              <a-button type="primary" @click="searchReset" icon="reload" style="margin-left: 8px">重置</a-button>
              <a @click="handleToggleSearch" style="margin-left: 8px">
                {{ toggleSearchStatus ? '收起' : '展开' }}
                <a-icon :type="toggleSearchStatus ? 'up' : 'down'"/>
              </a>
            </span>
          </a-col>
        </a-row>
      </a-form>
    </div>
    <!-- 查询区域-END -->

    <!-- 操作按钮区域 -->
    <div class="table-operator">
      <a-button @click="handleAdd" type="primary" icon="plus">新增</a-button>
      <a-button type="primary" icon="download" @click="handleExportXls('user_sale_prod_order')">导出</a-button>
      <a-upload name="file" :showUploadList="false" :multiple="false" :headers="tokenHeader" :action="importExcelUrl" @change="handleImportExcel">
        <a-button type="primary" icon="import">导入</a-button>
      </a-upload>
      <!-- 高级查询区域 -->
      <j-super-query :fieldList="superFieldList" ref="superQueryModal" @handleSuperQuery="handleSuperQuery"></j-super-query>
      <a-dropdown v-if="selectedRowKeys.length > 0">
        <a-menu slot="overlay">
          <a-menu-item key="1" @click="batchDel"><a-icon type="delete"/>删除</a-menu-item>
        </a-menu>
        <a-button style="margin-left: 8px"> 批量操作 <a-icon type="down" /></a-button>
      </a-dropdown>
    </div>

    <!-- table区域-begin -->
    <div>
      <div class="ant-alert ant-alert-info" style="margin-bottom: 16px;">
        <i class="anticon anticon-info-circle ant-alert-icon"></i> 已选择 <a style="font-weight: 600">{{ selectedRowKeys.length }}</a>项
        <a style="margin-left: 24px" @click="onClearSelected">清空</a>
      </div>

      <a-table
        ref="table"
        size="middle"
        :scroll="{x:true}"
        bordered
        rowKey="id"
        :columns="columns"
        :dataSource="dataSource"
        :pagination="ipagination"
        :loading="loading"
        :rowSelection="{selectedRowKeys: selectedRowKeys, onChange: onSelectChange}"
        class="j-table-force-nowrap"
        @change="handleTableChange">

        <template slot="htmlSlot" slot-scope="text">
          <div v-html="text"></div>
        </template>
        <template slot="imgSlot" slot-scope="text">
          <span v-if="!text" style="font-size: 12px;font-style: italic;">无图片</span>
          <img v-else :src="getImgView(text)" height="25px" alt="" style="max-width:80px;font-size: 12px;font-style: italic;"/>
        </template>
        <template slot="fileSlot" slot-scope="text">
          <span v-if="!text" style="font-size: 12px;font-style: italic;">无文件</span>
          <a-button
            v-else
            :ghost="true"
            type="primary"
            icon="download"
            size="small"
            @click="downloadFile(text)">
            下载
          </a-button>
        </template>

        <span slot="action" slot-scope="text, record">
          <a @click="handleEdit(record)">编辑</a>

          <a-divider type="vertical" />
          <a-dropdown>
            <a class="ant-dropdown-link">更多 <a-icon type="down" /></a>
            <a-menu slot="overlay">
              <a-menu-item>
                <a @click="handleDetail(record)">详情</a>
              </a-menu-item>
              <a-menu-item>
                <a-popconfirm title="确定删除吗?" @confirm="() => handleDelete(record.id)">
                  <a>删除</a>
                </a-popconfirm>
              </a-menu-item>
            </a-menu>
          </a-dropdown>
        </span>

      </a-table>
    </div>

    <user-sale-prod-order-modal ref="modalForm" @ok="modalFormOk"></user-sale-prod-order-modal>
  </a-card>
</template>

<script>

  import '@/assets/less/TableExpand.less'
  import { mixinDevice } from '@/utils/mixin'
  import { JeecgListMixin } from '@/mixins/JeecgListMixin'
  import UserSaleProdOrderModal from './modules/UserSaleProdOrderModal'
  import {filterMultiDictText} from '@/components/dict/JDictSelectUtil'

  export default {
    name: 'UserSaleProdOrderList',
    mixins:[JeecgListMixin, mixinDevice],
    components: {
      UserSaleProdOrderModal
    },
    data () {
      return {
        description: 'user_sale_prod_order管理页面',
        // 表头
        columns: [
          {
            title: '#',
            dataIndex: '',
            key:'rowIndex',
            width:60,
            align:"center",
            customRender:function (t,r,index) {
              return parseInt(index)+1;
            }
          },
          {
            title:'订单id',
            align:"center",
            dataIndex: 'saleProdOrderId'
          },
          {
            title:'用户id',
            align:"center",
            dataIndex: 'userId'
          },
          {
            title:'销售品id',
            align:"center",
            dataIndex: 'saleProdId'
          },
          {
            title:'合同id',
            align:"center",
            dataIndex: 'contractId'
          },
          {
            title:'订单状态',
            align:"center",
            dataIndex: 'orderStatus_dictText'
          },
          {
            title:'开始时间',
            align:"center",
            dataIndex: 'beginTime'
          },
          {
            title:'结束时间',
            align:"center",
            dataIndex: 'endTime'
          },
          {
            title:'创建时间',
            align:"center",
            sorter: true,
            dataIndex: 'createDate'
          },
          {
            title:'更新时间',
            align:"center",
            dataIndex: 'modifyDate'
          },
          {
            title:'支付时间',
            align:"center",
            dataIndex: 'payTime'
          },
          {
            title:'产品类型',
            align:"center",
            sorter: true,
            dataIndex: 'productType_dictText'
          },
          {
            title:'计费单元',
            align:"center",
            dataIndex: 'currencyUnit_dictText'
          },
          {
            title:'金额',
            align:"center",
            dataIndex: 'chargeMoney'
          },
          {
            title:'真实支付金额',
            align:"center",
            dataIndex: 'realPayMoney'
          },
          {
            title:'支付类型',
            align:"center",
            dataIndex: 'payType_dictText'
          },
          {
            title:'支付渠道',
            align:"center",
            dataIndex: 'payChannel'
          },
          {
            title:'支付者ID',
            align:"center",
            dataIndex: 'payId'
          },
          {
            title:'支付者IP',
            align:"center",
            dataIndex: 'payUserIpaddr'
          },
          {
            title:'是否有拓展值',
            align:"center",
            dataIndex: 'hasAttr_dictText'
          },
          {
            title:'appId',
            align:"center",
            dataIndex: 'appId'
          },
          {
            title:'备注',
            align:"center",
            dataIndex: 'remarks'
          },
          {
            title:'nmsc_streaming_no',
            align:"center",
            dataIndex: 'nmscStreamingNo'
          },
          {
            title:'nmsc_product_id',
            align:"center",
            dataIndex: 'nmscProductId'
          },
          {
            title:'是否新用户',
            align:"center",
            dataIndex: 'isNewCustomer_dictText'
          },
          {
            title:'关联订单id',
            align:"center",
            dataIndex: 'relOrderid'
          },
          {
            title:'是否开发票',
            align:"center",
            dataIndex: 'issueinvoice'
          },
          {
            title:'iap_receipt_verify_resp',
            align:"center",
            dataIndex: 'iapReceiptVerifyResp'
          },
          {
            title:'iap_transaction_receipt',
            align:"center",
            dataIndex: 'iapTransactionReceipt'
          },
          {
            title:'拓展字段1',
            align:"center",
            dataIndex: 'extFields1'
          },
          {
            title:'拓展字段2',
            align:"center",
            dataIndex: 'extFields2'
          },
          {
            title: '操作',
            dataIndex: 'action',
            align:"center",
            fixed:"right",
            width:147,
            scopedSlots: { customRender: 'action' }
          }
        ],
        url: {
          list: "/order/userSaleProdOrder/list",
          delete: "/order/userSaleProdOrder/delete",
          deleteBatch: "/order/userSaleProdOrder/deleteBatch",
          exportXlsUrl: "/order/userSaleProdOrder/exportXls",
          importExcelUrl: "order/userSaleProdOrder/importExcel",
          
        },
        dictOptions:{},
        superFieldList:[],
      }
    },
    created() {
    this.getSuperFieldList();
    },
    computed: {
      importExcelUrl: function(){
        return `${window._CONFIG['domianURL']}/${this.url.importExcelUrl}`;
      },
    },
    methods: {
      initDictConfig(){
      },
      getSuperFieldList(){
        let fieldList=[];
        fieldList.push({type:'BigDecimal',value:'saleProdOrderId',text:'订单id',dictCode:''})
        fieldList.push({type:'int',value:'userId',text:'用户id',dictCode:''})
        fieldList.push({type:'string',value:'saleProdId',text:'销售品id',dictCode:''})
        fieldList.push({type:'int',value:'contractId',text:'合同id',dictCode:''})
        fieldList.push({type:'list_multi',value:'orderStatus',text:'订单状态',dictTable:'', dictText:'', dictCode:'order_status'})
        fieldList.push({type:'datetime',value:'beginTime',text:'开始时间'})
        fieldList.push({type:'datetime',value:'endTime',text:'结束时间'})
        fieldList.push({type:'datetime',value:'createDate',text:'创建时间'})
        fieldList.push({type:'datetime',value:'modifyDate',text:'更新时间'})
        fieldList.push({type:'datetime',value:'payTime',text:'支付时间'})
        fieldList.push({type:'int',value:'productType',text:'产品类型',dictCode:'product_type'})
        fieldList.push({type:'int',value:'currencyUnit',text:'计费单元',dictCode:'currency_unit'})
        fieldList.push({type:'int',value:'chargeMoney',text:'金额',dictCode:''})
        fieldList.push({type:'int',value:'realPayMoney',text:'真实支付金额',dictCode:''})
        fieldList.push({type:'int',value:'payType',text:'支付类型',dictCode:'pay_type'})
        fieldList.push({type:'string',value:'payChannel',text:'支付渠道',dictCode:''})
        fieldList.push({type:'string',value:'payId',text:'支付者ID',dictCode:''})
        fieldList.push({type:'string',value:'payUserIpaddr',text:'支付者IP',dictCode:''})
        fieldList.push({type:'int',value:'hasAttr',text:'是否有拓展值',dictCode:'yes_or_no'})
        fieldList.push({type:'BigDecimal',value:'appId',text:'appId',dictCode:''})
        fieldList.push({type:'string',value:'remarks',text:'备注',dictCode:''})
        fieldList.push({type:'string',value:'nmscStreamingNo',text:'nmsc_streaming_no',dictCode:''})
        fieldList.push({type:'string',value:'nmscProductId',text:'nmsc_product_id',dictCode:''})
        fieldList.push({type:'int',value:'isNewCustomer',text:'是否新用户',dictCode:'yes_or_no'})
        fieldList.push({type:'BigDecimal',value:'relOrderid',text:'关联订单id',dictCode:''})
        fieldList.push({type:'int',value:'issueinvoice',text:'是否开发票',dictCode:''})
        fieldList.push({type:'string',value:'iapReceiptVerifyResp',text:'iap_receipt_verify_resp',dictCode:''})
        fieldList.push({type:'string',value:'iapTransactionReceipt',text:'iap_transaction_receipt',dictCode:''})
        fieldList.push({type:'string',value:'extFields1',text:'拓展字段1',dictCode:''})
        fieldList.push({type:'string',value:'extFields2',text:'拓展字段2',dictCode:''})
        this.superFieldList = fieldList
      }
    }
  }
</script>
<style scoped>
  @import '~@assets/less/common.less';
</style>