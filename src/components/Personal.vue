<template>
  <div class="login-wrap">
    <el-button class="AddProject" type="primary" @click="addproject">添加项目</el-button>
    <el-button class="LoginOut" type="warning" @click="loginOut">登出</el-button>

    <el-table
      :data="tableData"
      border
      ref="multipleTable"
      size="medium"
      class="container"
      header-cell-class-name="table-header"
      max-height="400px"
      @selection-change="handleSelectionChange">

      <el-table-column type="selection" width="55" align="center"></el-table-column>

      <el-table-column prop="id" label="项目id" width="100" align="center">
      </el-table-column>

      <el-table-column prop="address" label="项目名称"  width="450" align="center" show-overflow-tooltip>
      </el-table-column>

      <el-table-column label="操作" width="350" align="center">
        <template slot-scope="scope">
          <el-button
            size="small"
            type="primary"
            icon="el-icon-edit"
            @click="handleEdit(scope.$index, scope.row)"
          >编辑</el-button>
          <el-button
            size="small"
            type="primary"
            icon="el-icon-upload"
            @click="handlesub(scope.$index, scope.row)"
          >上传新版本</el-button>
          <el-button
            size="small"
            type="danger"
            icon="el-icon-delete"
            class="red"
            @click="handleDelete(scope.$index, scope.row)"
          >删除</el-button>
        </template>
      </el-table-column>
    </el-table>

    <Uploaddia :dialogVisible="Visible" @childFn="receive"></Uploaddia>
    <Newupload :dialogVisible="Visible1" @childFn="receive1"></Newupload>

  </div>
</template>

<script>
import Vue, {ref} from "vue";
import Uploaddia from "./Uploaddia"
import Newupload from "./Newupload";

export default {
  name: "Personal",
  components:{
    Uploaddia,
    Newupload
  },
  mounted:function() {
    console.log("username=%s,password=%s", this.userid, this.name);
    this.userid = sessionStorage.getItem("user_id");
    this.token = sessionStorage.getItem("token");
    let url='/project/list';
    let url1='/project/list?user_id=%3C'+this.userid+'%3E&token=%3C'+this.token+'%3E';

    //定义请求参数
    let params = {
      userid: this.userid,
      token: this.token
    };
    console.log(params);

    const GetOk = (res) => {
      if (res.data) {
        console.log(res.data.resp.status_code);
        if (res.data.resp.status_code === 0) {
          //判断返回的是否成功
          let project = {pro_id:'',name:''};
          for(project in res.data.project_list){
            this.tableData.append(project);
          }
          Vue.prototype.$message.success("创建成功！");
          this.$emit("childFn");

        } else {
          Vue.prototype.$message.error("内容出错！")
        }
      } else {
        Vue.prototype.$message.error("网络错误，请稍后再试")
      }
    }

    //发起ajax请求-Post（注意参数必须保存到params属性中）
    this.$axios.get(url1)
      .then(res => GetOk(res))
      .catch(err => {
        Vue.prototype.$message.error("请检查网络状况")
        console.error(err);
        //console.log(err);
      });
  },
  data(){
    return {
      tableData:[{id:'1',address:'20221108'}],
      Visible: false,
      Visible1: false,
    }
  },
  methods:{
    receive(){
      this.Visible=false
    },
    receive1(){
      this.Visible1=false
    },
    addproject:function (){
      this.Visible = ref(true);
    },
    handlesub:function (index, row){
      sessionStorage.removeItem('pro_id');
      sessionStorage.setItem("pro_id",this.tableData[index].id);
      this.Visible1 = ref(true);
    },
    loginOut:function(){
      this.$router.push('/Login');
      sessionStorage.clear();
      Vue.prototype.$message.success("已登出")
    },
    handleEdit:function (){
      this.$router.push('/Project');
    },
    // 删除用户
    handleDelete(index, row) {
      this.$confirm('确定要删除吗？', '提示', {
        type: 'warning'
      })
        .then(() => {

          console.log("id:"+this.tableData[index].id);

          let params={
            user_id: '',
            pro_id: '',
            token: ''
          };
          params.pro_id=this.tableData[index].id;
          params.token=sessionStorage.getItem("token");
          params.user_id=sessionStorage.getItem("user_id");


          this.$axios.post("/project/delete",
            params
          ).then((res)=>{
            if(res.data){
              this.$message.success('删除成功');//记住
            }else{
              this.$message.warning('删除失败')
            }

          }).catch(function (error) {
            console.log("服务器连接异常！")
          })

          this.tableData.splice(index, 1);

        })
        .catch(() => {});
    },

  }
}

</script>

<style scoped>
.login-wrap{
  border-radius: 10px;
  margin: 20px auto;
  width: 1050px;
  height: 660px;
  padding: 30px 35px 15px 35px;
  background: #fff;
  border: 1px solid #eaeaea;
  text-align: left;
  box-shadow: 0 0 20px 2px rgba(0, 0, 0, 0.1);
}
.AddProject{
  position: relative;left:180px;top:0px;
  font-weight: 600;
  font-size: 14px;
}
.DelProject{
   position: relative;left:340px;top:0px;
   font-weight: 600;
   font-size: 14px;
 }
.LoginOut{
  position: relative;left:600px;top:0px;
  font-weight: 600;
  font-size: 14px;
}
.container {
  border-radius: 10px;
  margin: 50px auto;
  width: 1050px;
  height: 600px;
  padding: 30px 35px 15px 35px;
  background: #fff;
  border: 1px solid #eaeaea;
  text-align: left;
  box-shadow: 0 0 20px 2px rgba(0, 0, 0, 0.1);
}
</style>
