<template>
  <div>
    <!-- 
        inline:行内表单，代表一行可以放置多个元素
     -->
<el-form :inline="true" class="demo-form-inline" :model="cForm">
  <el-form-item label="一级分类" >
        <el-select placeholder="请选择" v-model="cForm.category1Id" @change="handler1" :disabled="show">
        <el-option :label="c1.name" :value="c1.id" v-for="(c1,index) in list1" :key="c1.id"></el-option>
        </el-select>
  </el-form-item>
    <el-form-item label="二级分类">
        <el-select placeholder="请选择" v-model="cForm.category2Id" @change="handler2" :disabled="show">
        <el-option :label="c2.name" :value="c2.id" v-for="(c2,index) in list2" :key="c2.id"></el-option>
        </el-select>
    </el-form-item>
    <el-form-item label="三级分类">
        <el-select placeholder="请选择" v-model="cForm.category3Id" @change="handler3" :disabled="show">
        <el-option :label="c3.name" :value="c3.id" v-for="(c3,index) in list3" :key="c3.id"></el-option>
        </el-select>
    </el-form-item>
</el-form>
  </div>
</template>

<script>
export default {
    name:'categorySelected',
    data(){
      return {
        //一级分类数据
        list1:[],
        list2:[],
        list3:[],
        categoryList:[],
        //收集相应的id
        cForm:{
          category1Id:'',
          category2Id:'',
          category3Id:'',
        }
      }
    },
    props:['show'],
    mounted(){
      //获取一级菜单的数据
      this.getCategoryList()
    },
    methods:{
      async getCategoryList(){
       let result=await this.$API.attr.reqGetCategory1()
        if(result.code==200){
          this.list1=result.data
        }
      },
      async handler1(){
        //清除数据
        this.list2=[]
        this.list3=[]
        this.cForm.category2Id=''
        this.cForm.category3Id=''
        //一级分类回调 option 发生变化时 触发
        const {category1Id}= this.cForm
        this.$emit('getCategoryId',{categoryId:category1Id,level:1})
        let result=await this.$API.attr.reqGetCategory2(category1Id)
        if(result.code==200){
          this.list2=result.data
        }
      },
      async handler2(){
          //清除数据
        this.list3=[]
        this.cForm.category3Id=''
        //二级分类回调
        const {category2Id}= this.cForm
        this.$emit('getCategoryId',{categoryId:category2Id,level:2})
        let result=await this.$API.attr.reqGetCategory3(category2Id)
        if(result.code==200){
          this.list3=result.data
        }
      },
      handler3(){
        //三级分类回调
        const {category3Id} =this.cForm
        this.$emit('getCategoryId',{categoryId:category3Id,level:3})
      }
    }
}
</script>

<style>

</style>