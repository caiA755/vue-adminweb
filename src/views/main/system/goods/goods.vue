<template>
  <div class="goods">
     <div class="menusearch search">
       <caiform :formItems="formItems" v-model="formData">
          <template #search>
            <el-button  type="primary" :icon="el-icon-refresh">重置</el-button>
            <el-button type="primary" :icon="Search">查询</el-button>
          </template>
        </caiform>
   </div>
   <div class="maincontent">
   <!-- <div>{{tableData}}</div> -->
     <mytable :tableData="tableData" :columns="columns"   :pageParams="pageParams"  :@handleSizeChange="handleSizeChange" :@handleCurrentChange="handleCurrentChange">
        <template #objectId>
            <el-button type="success" plain>编辑</el-button>
            <el-button type="success">删除</el-button>
        </template>
     </mytable>
   </div>
  </div>
</template>

<script>
import caiform from '@/base-ui/index'
import mytable from '@/components/my_table/index'
import cRequest1 from '@/service/index'
import {ref} from 'vue'
import mainurl from '@/service/url/mainurl'

export default {
  name: 'goods',
    components:{
    caiform,
    mytable
  },
   data(){
     let tableData;
    let columns=[
     {
       prop:"objectId",
       name:"objectId"
     },
     {
       prop:"name",
       name:"商品"
     },
      {
       prop:"descri",
       name:"描述"
     },
      {
       prop:"category_id",
       name:"类别",
       children:"name"
     },
     {
       prop:"createdAt",
       name:"创建时间",
     },
     {
       prop:"updatedAt",
       name:"更新时间"
     }
     ,
     {
       prop:"objectId",
       name:"操作",
       slotname:"objectId",
       minWidth:300
     }
    ];
    let pageParams={
          page:1,
          limit:5,
          total:7
    }
    return {tableData,columns,pageParams}
  },
  created(){
        let _this=this;
        cRequest1.getRequest(mainurl.goodsUrl+"?include=category_id",null).then((res)=>{
             _this.tableData=res.results;
       });
  },
  setup() {
    const formItems=[
      {
        field:"name",
        label:"名称",
        placeholder:"请输入商品名称",
        type:"input"
      }  ,
      {
        field:"date",
        label:"创建时间",
        placeholder:"请选择创建时间范围",
        type:"datepicker",
        otherOptions:{
          startPlaceholder:'开始时间',
          endPlaceholder:'结束时间',
          type:"daterange"
        }
       }
    ];
    const formData=ref({
          date:"",
          name:""
     })
    return {formItems,formData}
  }
}
</script>


<style scoped></style>
