<template>

  <!--修改用户权限-->
  <el-dialog title="修改用户信息" :visible.sync="editRulesDialogVisible" width="50%" @close="editRulesDialogClosed">
    <el-form :model="editRulesForm" :rules="editRulesFormRules" ref="editRulesFormRef" lable-width="70px">

      <el-form-item label="权限分配">
        <el-radio v-model="editRulesForm.role" border label="普通用户"></el-radio>
        <el-radio v-model="editRulesForm.role"  border label="超级管理员"></el-radio>
      </el-form-item>

    </el-form>

    <!--内容底部区域-->
    <span slot="footer" class="dialog-footer">
        <el-button @click="editRulesDialogVisible = false">取 消</el-button>
        <el-button type="primary" @click="editRules">确 认</el-button>
      </span>
  </el-dialog>

  // 权限信息
  editRulesForm:{
  role:"",
  id: -1
  },
  // 权限对话框状态
  editRulesDialogVisible:false,

  // 显示对话框-修改权限
  async showEditRulesDialog(id, role){
  this.editRulesForm.role = role;
  this.editRulesForm.id = id;
  this.editRulesDialogVisible = true; // 开启对话框-修改信息
  },
  // 关闭窗口-修改权限
  editRulesDialogClosed(){
  this.$refs.editRulesFormRef.resetFields(); // 重置信息
  },
  // 确认修改-修改权限
  editRules(){
  this.$refs.editRulesFormRef.validate(async valid => {
  if(!valid) return;
  // 发起修改请求
  const {data:res} = await this.$http.put("editRules", this.editRulesForm);
  if(res!="success"){
  return this.$message.error("修改失败");
  }
  this.$message.success("修改成功");
  // 隐藏对话框-修改信息
  this.editRulesDialogVisible = false;
  this.getUserList();
  });
  },

  public int editRules(User user);

  @RequestMapping("/editRules")
  public String editRules(@RequestBody User user){
  int i = udao.editRules(user);
  return i > 0 ? "success" : "fail";
  }

  <update id="editRules">
    UPDATE  easyuser SET role = #{role} WHERE id = #{id}
  </update>

</template>

<script>
export default {
  name: "editRules"
}
</script>

<style scoped>

</style>