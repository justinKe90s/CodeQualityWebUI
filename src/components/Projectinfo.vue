<template>
  <div>
    <el-card>
      <el-descriptions title="项目详细信息" :column="2" size="medium" :border='true'>
        <el-descriptions-item>
          <template slot="label">
            版本id
          </template>
          {{ ver_id }}
        </el-descriptions-item>
        <el-descriptions-item>
          <template slot="label">
            上传日期
          </template>
          {{ time }}
        </el-descriptions-item>
        <el-descriptions-item>
          <template slot="label">
            原始文件哈希值
          </template>
          {{ hash }}
        </el-descriptions-item>
        <el-descriptions-item>
          <template slot="label">
            代码行数
          </template>
          {{ length }}
        </el-descriptions-item>
        <el-descriptions-item>
          <template slot="label">
            调用图id
          </template>
          {{ call_graph_id }}
        </el-descriptions-item>
        <el-descriptions-item>
          <template slot="label">
            文件个数
          </template>
          {{ num_files }}
        </el-descriptions-item>
        <el-descriptions-item>
          <template slot="label">
            类的个数
          </template>
          {{ num_classes }}
        </el-descriptions-item>
        <el-descriptions-item>
          <template slot="label">
            方法个数
          </template>
          {{ num_methods }}
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
            内部边数
          </template>
          {{ num_edges }}
        </el-descriptions-item>
        <el-descriptions-item>
          <template slot="label">
            接口数
          </template>
          {{ num_api }}
        </el-descriptions-item>
        <el-descriptions-item>
          <template slot="label">
            有注释的接口数
          </template>
          {{ num_comment_api }}
        </el-descriptions-item>
        <el-descriptions-item>
          <template slot="label">
            接口参数总数
          </template>
          {{ num_api_parameters }}
        </el-descriptions-item>
        <el-descriptions-item>
          <template slot="label">
            调用层次（平均值）
          </template>
          {{ call_hierarchy }}
        </el-descriptions-item>
        <el-descriptions-item>
          <template slot="label">
            内聚度
          </template>
          {{ coupling }}
        </el-descriptions-item>
        <el-descriptions-item>
          <template slot="label">
            圈复杂度
          </template>
          {{ cyclomatic_complexity }}
        </el-descriptions-item>
        <el-descriptions-item>
          <template slot="label">
            可修改性
          </template>
          {{ modifiability }}
        </el-descriptions-item>
        <el-descriptions-item>
          <template slot="label">
            可扩展性
          </template>
          {{ extensibility }}
        </el-descriptions-item>
        <el-descriptions-item>
          <template slot="label">
            易测试性
          </template>
          {{ testability }}
        </el-descriptions-item>
        <el-descriptions-item>
          <template slot="label">
            可替换性
          </template>
          {{ replaceability }}
        </el-descriptions-item>
        <el-descriptions-item>
          <template slot="label">
            易理解性
          </template>
          {{ understandability }}
        </el-descriptions-item>
        <el-descriptions-item>
          <template slot="label">
            所属项目id
          </template>
          {{ pro_id }}
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
  name: "Projectinfo",
  data() {
    return {
      ver_id: Number,
      time: Number,
      hash: String,
      length: Number,
      call_graph_id: Number,
      num_files: Number,
      num_classes: Number,
      num_methods: Number,
      num_valid_lines: Number,
      num_comment_lines: Number,
      num_blank_lines: Number,
      num_edges: Number,
      num_api: Number,
      num_comment_api: Number,
      num_api_parameters: Number,
      call_hierarchy: Number,
      coupling: Number,
      cyclomatic_complexity: Number,
      modifiability: Number,
      extensibility: Number,
      testability: Number,
      replaceability: Number,
      understandability: Number,
      pro_id: Number,
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
      sessionStorage.clear();
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
