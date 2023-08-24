<template>
  <div class="table-box">
    <div class="title">
      <h2>简单的CRUD demo</h2>
    </div>
    <!-- 导航 -->
    <div class="query-box">
      <el-input
        class="query-input"
        v-model="queryInputer"
        placeholder="请输入姓名搜索🔍"
      />
      <div class="btn-list">
        <el-button
          type="danger"
          @click="handleDelList"
          v-if="multipleSelection.length > 0"
          >删除多选</el-button
        >
        <el-button type="primary" @click="handleAdd">增加</el-button>
      </div>
    </div>

    <!-- 表格 -->
    <el-table
      border
      ref="multipleTableRef"
      :data="tableData"
      style="width: 100%"
      @selection-change="handleSelectionChange"
    >
      <el-table-column type="selection" width="55" />
      <el-table-column fixed prop="name" label="姓名" width="150" />
      <el-table-column prop="email" label="邮箱" width="120" />
      <el-table-column prop="phone" label="电话" width="120" />
      <el-table-column prop="state" label="C状态" width="120" />
      <el-table-column prop="address" label="地址" width="400" />
      <el-table-column fixed="right" label="Operations" width="120">
        <template #default="scope">
          <el-button
            link
            type="primary"
            size="small"
            @click="handleRowDel(scope.row)"
            style="color: #f56c6c"
            >删除</el-button
          >
          <el-button
            link
            type="primary"
            size="small"
            @click="handleEdit(scope.row)"
            >编辑</el-button
          >
        </template>
      </el-table-column>
    </el-table>

    <!-- 添加内容页面 -->
    <el-dialog
      v-model="dialogFormVisible"
      :title="dialogType === 'add' ? '新增' : '编辑'"
    >
      <el-form :model="tableForm">
        <el-form-item label="姓名" :label-width="60">
          <el-input v-model="tableForm.name" autocomplete="off" />
        </el-form-item>
        <el-form-item label="邮箱" :label-width="60">
          <el-input v-model="tableForm.email" autocomplete="off" />
        </el-form-item>
        <el-form-item label="电话" :label-width="60">
          <el-input v-model="tableForm.phone" autocomplete="off" />
        </el-form-item>
        <el-form-item label="状态" :label-width="60">
          <el-input v-model="tableForm.state" autocomplete="off" />
        </el-form-item>
        <el-form-item label="地址" :label-width="60">
          <el-input v-model="tableForm.address" autocomplete="off" />
        </el-form-item>
      </el-form>
      <template #footer>
        <span class="dialog-footer">
          <el-button type="primary" @click="dialogConfirm"> 确认 </el-button>
        </span>
      </template>
    </el-dialog>
  </div>
</template>

<script setup>
import { ITEM_RENDER_EVT } from "element-plus/es/components/virtual-list/src/defaults";

// import { ref } from "vue";

// 数据
let queryInput = $ref("");
let tableData = $ref([
  {
    id: "1",
    name: "Tom1",
    email: "123@qq.com",
    phone: "1380013800",
    state: "California",
    address: "No. 189, Grove St, Los Angeles",
  },
  {
    id: "2",
    name: "Tom2",
    email: "123@qq.com",
    phone: "1380013800",
    state: "California",
    address: "No. 189, Grove St, Los Angeles",
  },
  {
    id: "3",
    name: "Tom3",
    email: "123@qq.com",
    phone: "1380013800",
    state: "California",
    address: "No. 189, Grove St, Los Angeles",
  },
  {
    id: "4",
    name: "Tom4",
    email: "123@qq.com",
    phone: "1380013800",
    state: "California",
    address: "No. 189, Grove St, Los Angeles",
  },
]);
let multipleSelection = $ref([]);
let dialogFormVisible = $ref(false);
let tableForm = $ref({
  name: "朱某",
  email: "2905074035@qq.com",
  phone: "19516715997",
  state: "在校生",
  address: "鼠科大",
});
let dialogType = $ref("add");

// 方法

// 编辑
const handleEdit = (row) => {
  dialogFormVisible = true;
  dialogType = "edit";
  tableForm = { ...row };
};

// 删除
const handleRowDel = ({ id }) => {
  // console.log(id);
  // 通过id获取到条目对应的索引值
  let index = tableData.findIndex((item) => item.id === id);
  // 通过索引值进行删除对应条目
  tableData.splice(index, 1); //splice使用方法
};

// 多选删除
const handleDelList = () => {
  multipleSelection.forEach((id) => {
    handleRowDel({ id });
  });
};
multipleSelection = [];

// 多选
const handleSelectionChange = (val) => {
  multipleSelection = [];
  // console.log(val);
  val.forEach((item) => {
    multipleSelection.push(item.id);
  });
};

// 添加
const handleAdd = () => {
  dialogFormVisible = true; //打开弹窗
  tableForm = {};
  dialogType = "add";
};

// 确认
const dialogConfirm = () => {
  dialogFormVisible = false; //关闭弹窗
  if (dialogType === "add") {
    tableData.push({
      id: tableData.length + 1,
      ...tableForm,
    });
  } else {
    let index = tableData.findIndex((item) => item.id === tableForm.id);
    tableData[index] = tableForm;
  }
};
</script>

<style scoped>
.table-box {
  width: 800px;
  margin: 200px auto;
}
.title {
  text-align: center;
}
.query-box {
  display: flex;
  justify-content: space-between;
  margin-bottom: 20px;
}

.query-input {
  width: 140px;
}

.el-input {
  width: 300px;
}
</style>
