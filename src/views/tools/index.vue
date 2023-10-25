<template>
  <div style="width: 800px; margin: 20px auto">
    <el-button icon="Refresh" type="primary" @click="handleReset" style="margin-bottom: 20px">重置</el-button>
    <el-table :data="tableData" border style="width: 100%" show-summary :summary-method="getSummaries">
      <el-table-column width="120" align="center">
        <template #default>
          <el-button icon="Plus" type="primary" @click="handleAdd">新增</el-button>
        </template>
      </el-table-column>
      <el-table-column width="100" align="center">
        <template #default="scope">
          <el-select v-model="scope.row.method" filterable style="width: 100%">
            <el-option v-for="(item, index) in options" v-bind="item" :key="index" placeholder="请选择" />
          </el-select>
        </template>
      </el-table-column>
      <el-table-column label="数量" align="center">
        <template #default="scope">
          <el-input-number v-model="scope.row.num" style="width: 100%" />
        </template>
      </el-table-column>
      <el-table-column width="100" align="center">
        <template #default="scope">
          <el-select v-model="scope.row.method2" filterable style="width: 100%">
            <el-option v-for="(item, index) in options2" v-bind="item" :key="index" placeholder="请选择" />
          </el-select>
        </template>
      </el-table-column>
      <el-table-column label="价格" align="center">
        <template #default="scope">
          <el-input-number v-model="scope.row.price" style="width: 100%" />
        </template>
      </el-table-column>
      <el-table-column width="120" align="center">
        <template #default="scope">
          <el-button icon="Delete" type="primary" @click="handleDelete(scope.$index)">删除</el-button>
        </template>
      </el-table-column>
    </el-table>
  </div>
</template>

<script lang="js" setup>
import { ref } from "vue"
import { ElMessage } from "element-plus"

const defaultValue = { method: "+", num: 1, price: 0, method2: "*" }
const tableData = ref([{ ...defaultValue }])
const options = ref([
  { label: "+", value: "+" },
  { label: "-", value: "-" }
])
const options2 = ref([
  { label: "乘", value: "*" },
  { label: "除", value: "÷" }
])

const handleAdd = () => {
  tableData.value.push({ ...defaultValue })
}

const handleDelete = (i) => {
  if (tableData.value.length === 1) {
    ElMessage.error("至少保有一条")
    return
  }
  tableData.value.splice(i, 1)
}

const getSummaries = () => {
  const total = tableData.value.reduce((prev, next) => {
    const res = next.method2 === "*" ? next.num * next.price : next.num / next.price
    return next.method === "+" ? prev + res : prev - res
  }, 0)
  return ["总计:", total]
}

const handleReset = () => {
  tableData.value = [{ ...defaultValue }]
}
</script>

<style>
html,
#app {
  background-color: #fff;
}
</style>
