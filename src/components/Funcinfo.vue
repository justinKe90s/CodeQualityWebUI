<template>
  <div>
    <el-card>
      <el-descriptions title="方法详细信息" :column="2" size="medium" :border='true'>
        <el-descriptions-item>
          <template slot="label">
            方法id
          </template>
          {{ mth_id }}
        </el-descriptions-item>
        <el-descriptions-item>
          <template slot="label">
            方法名
          </template>
          {{ name }}
        </el-descriptions-item>
        <el-descriptions-item>
          <template slot="label">
            返回值类型，参数名及其类型
          </template>
          {{ signature }}
        </el-descriptions-item>
        <el-descriptions-item>
          <template slot="label">
            代码行数（系统规模）
          </template>
          {{ length }}
        </el-descriptions-item>
        <el-descriptions-item>
          <template slot="label">
            有效代码行数
          </template>
          {{ num_valid_lines }}
        </el-descriptions-item>

        <el-descriptions-item>
          <template slot="label">
            注释行数
          </template>
          {{ num_comment_lines }}
        </el-descriptions-item>
        <el-descriptions-item>
          <template slot="label">
            空行个数
          </template>
          {{ num_blank_lines }}
        </el-descriptions-item>
        <el-descriptions-item>
          <template slot="label">
            参数个数
          </template>
          {{ num_parameters }}
        </el-descriptions-item>
        <el-descriptions-item>
          <template slot="label">
            入度
          </template>
          {{ num_inbound }}
        </el-descriptions-item>
        <el-descriptions-item>
          <template slot="label">
            出度
          </template>
          {{ num_outbound }}
        </el-descriptions-item>
        <el-descriptions-item>
          <template slot="label">
            调用层次
          </template>
          {{ call_hierarchy }}
        </el-descriptions-item>
        <el-descriptions-item>
          <template slot="label">
            圈复杂度
          </template>
          {{ cyclomatic_complexity }}
        </el-descriptions-item>
        <el-descriptions-item>
          <template slot="label">
            所属类id
          </template>
          {{ class_id }}
        </el-descriptions-item>
      </el-descriptions>
    </el-card>

    <el-button class="Back" type="primary" @click="back">返回</el-button>
    <el-button class="LoginOut" type="warning" @click="loginOut">登出</el-button>

  </div>
</template>

<script>
import Vue from "vue";

export default {
  name: "Funcinfo",
  data() {
    return {
      mth_id: Number,
      name: String,
      signature:String,
      length: Number,
      num_valid_lines: Number,
      num_comment_lines: Number,
      num_blank_lines: Number,
      num_parameters: Number,
      num_inbound: Number,
      num_outbound: Number,
      call_hierarchy: Number,
      cyclomatic_complexity: Number,
      class_id: Number
    };
  },
  mounted() {
    this.load();
  },
  methods: {
    load() {
      userInfo(this.$route.params.id)
        .then((res) => {
          this.avatar = res.data.avatar;
          this.account = res.data.account;
          this.age = res.data.age;
          this.email = res.data.email;
          this.mobilePhoneNumber = res.data.mobilePhoneNumber;
          this.area = res.data.area;
          this.createDate = res.data.createDate;
          this.nickname = res.data.nickname;
          this.sex = res.data.sex == 1 ? "男" : "女";
          this.work = res.data.work;
          this.design = res.data.design;
          this.hobby = res.data.hobby;
        })
        .catch((err) => {
          console.log(err);
        });
    },
    loginOut:function(){
      this.$router.push('/Login');
      Vue.prototype.$message.success("已登出")
    },
    back:function (){
      this.$router.push('/Project');
    }
  }
}
</script>

<style scoped>
.Back{
  position: relative;left:500px;top:20px;
  font-weight: 800;
  font-size: 16px;
}
.LoginOut{
  position: relative;left:720px;top:20px;
  font-weight: 800;
  font-size: 16px;
}

</style>
