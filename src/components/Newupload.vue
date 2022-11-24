<template>
  <el-dialog
    title="上传项目新版本"
    :visible.sync="dialogVisible"
    width="30%"
    top="15%"
    :append-to-body="true"
    @close="handleClose"
  >

    <input type="file" id="file" hidden @change="fileChange">
    <el-input placeholder="所上传文件" id="path" ref = "input1" v-bind:value="value" v-on:input="changeValue" class="input-with-select">
      <p>{{ value }}</p>
      <el-button slot="append" icon="el-icon-folder" type="success" @click="btnChange"></el-button>
    </el-input>

    <el-button style="margin-top: 20px;margin-left: 30%;" @click="Analyze">开始分析</el-button>

  </el-dialog>

</template>

<script>

import Vue, {ref} from "vue";

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
      value:'',
      tableData:[{id:'1',address:'20221108'},
        {id:'2',address:'20221118'}]
    }
  },
  methods: {
    changeValue(){
      this.value = this.$refs.input1.value;
    },
    Analyze(){

    },
    fileChange() {
      let f = document.getElementById("file");
      let url1 = '/project/upload?pro_id=%3C' + sessionStorage.getItem("pro_id")
        + '%3E&user_id=%3C' + sessionStorage.getItem("user_id")
        + '%3E&token=%3C' + sessionStorage.getItem("token") + '%3E';

      if (f.files) {
        let fileList = f.files;
        let formData = new FormData();  //创建空对象
        for (let i = 0; i < fileList.length; i++) {

          this.value = fileList[i].name;
          //如需判断文件就在这判断 现在不判断
          if(fileList[i].name.substring(fileList[i].name.lastIndexOf('.')+1)!=="rar"
            && fileList[i].name.substring(fileList[i].name.lastIndexOf('.')+1)!=="jar"
            && fileList[i].name.substring(fileList[i].name.lastIndexOf('.')+1)!=="java"){
            this.$message.error("文件类型错误！");
            return;
          }

          formData.append(fileList[i].name, fileList[i]);
          console.log(fileList[i].name)
          this.$refs.input1.value = fileList[i].name;
        }
        this.$axios.post(url1, formData)
          .then((res) => {
            if (res.data.resp.status_code === 0) {
              Vue.prototype.$message.success("上传成功！");
            } else {
              this.$message.error("获取数据失败，无法上传");
            }
          })
          .catch((err) => {
            console.log(err);
            this.$message.error("网络错误，请稍后再试");
          });
      }
    },
    btnChange() {
      document.getElementById('file').click()
    },
    show() {
      this.dialogVisible = ref(true)
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
