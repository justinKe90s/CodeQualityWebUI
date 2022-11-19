<template>
  <el-dialog
    title="上传项目新版本(上传后自动开始分析)"
    :visible.sync="dialogVisible"
    width="30%"
    top="15%"
    :append-to-body="true"
    @close="handleClose"
  >
    <el-button class="java" type="primary" @click="java">上传.java文件</el-button>
    <el-button class="rar" type="primary" @click="rar">上传.rar压缩包</el-button>
    <el-button class="jar" type="primary" @click="jar">上传jar包</el-button>

  </el-dialog>

</template>

<script>

import {ref} from "vue";

export default {
  name: 'Uploaddia',
  props: {
    dialogVisible:{
      type:Boolean,
      default:false,
    }
  },
  data() {
    return {
      tableData:[{id:'1',address:'20221108'},
        {id:'2',address:'20221118'}]
    }
  },
  methods: {
    show() {
      this.dialogVisible = ref(true)
    },
    See:function (){

    },
    rar:function (){

    },
    jar:function (){

    },
    java:function (){

    },
    handleClose() {
      this.$emit("childFn");
      //这个ChildFn就是自定义方法，如果要传递数据可以这样写
      //this.$emit("childFn",'传递的数据');
      //然后在父组件方法中就可以拿到了
      //receive(value){
      //console.log(value) 这个value就是传递的值
      //},
    }
  }
}
</script>

<style scoped>

</style>
