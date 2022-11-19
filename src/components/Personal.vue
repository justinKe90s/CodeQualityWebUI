<template>
  <div class="login-wrap">
    <el-button class="AddProject" type="primary" @click="addproject">添加项目</el-button>
    <el-button class="DelProject" type="danger" @click="delproject">批量删除</el-button>
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
  data(){
    return {
      tableData:[{id:'1',address:'HelloWorld'}],
      Visible: false,
      Visible1: false,
    }
  },


  methods:{
    getList(){
      let data={
        pageModel:{
          pageNo:this.pageNo,
          pageSize:this.limit
        },
        shop:{
          style:this.radio
        }
      }
      listShop(data).then(res =>{
        this.total = res.data.data.total
        this.lists = res.data.data.rows.filter(res=>{
          return res.isdelete == '0'
        })
      })
        //请求完列表后，回调，过滤需要被勾选的
        .then(()=>{
          for(let i=0;i<this.lists.length;i++){
            if(this.lists[i].style == this.radio){
              this.$refs.table.toggleRowSelection(this.lists[i],true);
            }
          }
        })
    },
    receive(){
      this.Visible=false
    },
    receive1(){
      this.Visible1=false
    },
    addproject:function (){
      this.Visible = ref(true);
    },
    delproject:function (){

    },
    handlesub:function (){
      this.Visible1 = ref(true);
    },
    loginOut:function(){
      this.$router.push('/Login');
      Vue.prototype.$message.success("已登出")
    },
    handleEdit:function (){
      this.$router.push('/Project');
    },
    showAllUsers:function (){
      this.tableData=[{id:'1',address:'HelloWorld.java'}]
      /*this.$axios.post("/allUsersInfo").then( (response)=> {
        this.tableData=response.data;
        this.allUsers=response.data;
        this.pageTotal=Math.ceil(this.allUsers.length/5);

        console.log(this.pageTotal);
      }).catch(function (error) {
        console.log("获取所有用户信息失败！")
      })

       */

    },
    handleSearch() {
      this.$axios.post("/allUsersInfo").then( (response)=> {

        let name1=document.getElementById('search').value;
        let department1=document.getElementById('department').value
        let data;
        if(name1!==""||department1!==""){

          if(name1==""&&department1!=="")
            //查找
            data = this.allUsers.find(item=>item.department==department1)
          else if(department1==""&&name1!=="")
            data = this.allUsers.find(item=>item.name==name1)
          else
            // (name1!==""&&department1!=="")
            data = this.allUsers.find(item=>item.name==name1&&item.department==department1)

          if(data == undefined){
            this.$message({
              showClose: true,
              message: 'no user'
            });
          }else{
            this.tableData=[data];//为什么要放在axios里面才有反应
          }
        } else {
          this.showAllUsers();
        }}).catch(function (error) {
        console.log("获取所有用户信息失败！")
      })
    },
    // 删除用户
    handleDelete(index, row) {
      this.$confirm('确定要删除吗？', '提示', {
        type: 'warning'
      })
        .then(() => {

          console.log("id:"+this.tableData[index].id);

          let params=new URLSearchParams();
          params.append("id",this.tableData[index].id);

          this.$axios.post("/deleteUser",
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

    //新增用户
    submmit(){

      let employeeId0=document.getElementById("employeeId").value;
      let name0=document.getElementById("name2").value;
      let sex0=document.getElementById("sex2").value;
      let department0=document.getElementById("department2").value;
      let birthday0=document.getElementById("birthday").value;
      let password="123456"
      let tel0=document.getElementById("tel").value;

      let params=new URLSearchParams();
      params.append("id",employeeId0);
      params.append("name",name0);
      params.append("sex",sex0);
      params.append("tel",tel0);
      console.log(tel0);
      params.append("department",department0);
      params.append("password",password);
      params.append("birthday",birthday0);

      this.$axios.post(
        "/newUser",
        params
      ).then((res)=>{
          console.log(res.data)
          if(res.data){
            this.$message({
              message:"增添成功",
              type:"success"
            });
          }else {
            this.$message("此Id已注册");
          }
        }
      ).catch(function (error) {
        console.log("错误！！")
      })
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
  position: relative;left:500px;top:0px;
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
