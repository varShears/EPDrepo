<template>
  <div>
    <el-container>
      <el-aside width="200" style="background-color: rgb(238, 241, 246)">
        <el-menu :default-active="menuActive" @select="menuSelect" style="width: 200px">
          <el-menu-item index="1">EPD网关发现</el-menu-item>
          <el-menu-item index="2">EPD管理</el-menu-item>
          <el-menu-item index="3">桌牌模板</el-menu-item>
        </el-menu>
      </el-aside>
      <el-main style="height:100vh">
        <el-table v-show="menuActive==='1'" :data="tableData">
          <el-table-column prop="sn" label="网关SN"></el-table-column>
          <el-table-column prop="ip" label="网关IP"></el-table-column>
          <el-table-column prop="port" label="端口"></el-table-column>
          <el-table-column prop="version" label="版本"></el-table-column>
          <el-table-column label="操作">
            <template scope="scope">
              <el-button type="primary" @click="openDialog(scope.row)">下发任务</el-button>
            </template>
          </el-table-column>
        </el-table>

        <el-table v-show="menuActive==='2'" :data="tableData2">
          <el-table-column prop="id" label="桌牌ID"></el-table-column>
          <el-table-column prop="name" label="名称"></el-table-column>
          <el-table-column prop="time" label="添加时间"></el-table-column>
          <el-table-column label="操作">
            <template scope="scope">
              <el-button type="primary" @click="deleteMsg(scope.row)">删除</el-button>
            </template>
          </el-table-column>
        </el-table>
        <el-button
          style="margin: 20px"
          v-show="menuActive==='2'"
          type="primary"
          @click="addTable"
        >添加桌牌</el-button>
      </el-main>
    </el-container>
    <el-dialog title="下发桌牌任务" :visible.sync="dialogVisible" width="30%" :before-close="handleClose">
      <el-form :model="putForm" label-width="80px">
        <el-form-item label="EPD编号">
          <el-select v-model="putForm.code" placeholder="请选择">
            <el-option
              v-for="(item,index) in tableData2"
              :key="index"
              :value="item.id"
              :label="item.name"
            ></el-option>
          </el-select>
        </el-form-item>
        <el-form-item label="模板">
          <el-select v-model="putForm.moduleSelection" placeholder="请选择">
            <!-- <el-option v-for="" :key=""></el-option> -->
            <el-option label="test" value="test"></el-option>
          </el-select>
        </el-form-item>
        <el-form-item label="显示内容">
          <el-input v-model="putForm.content" placeholder></el-input>
        </el-form-item>
        <el-form-item label="开始时间">
          <el-date-picker type="datetime" v-model="putForm.startTime" placeholder></el-date-picker>
        </el-form-item>
        <el-form-item label="结束时间">
          <el-date-picker type="datetime" v-model="putForm.endTime" placeholder></el-date-picker>
        </el-form-item>
      </el-form>
      <span slot="footer" class="dialog-footer">
        <el-button @click="dialogVisible = false">取 消</el-button>
        <el-button type="primary" @click="dialogVisible = false">确 定</el-button>
      </span>
    </el-dialog>
    <el-dialog
      title="添加EPD桌牌"
      :visible.sync="dialogVisibleT"
      width="30%"
      :before-close="handleClose"
    >
      <el-form :model="EPDForm">
        <el-form-item label="EPD编号">
          <el-input v-model="EPDForm.code"></el-input>
        </el-form-item>
        <el-form-item label="名称">
          <el-input v-model="EPDForm.name" placeholder></el-input>
        </el-form-item>
      </el-form>
    </el-dialog>
  </div>
</template>

<script>
export default {
  data() {
    return {
      menuActive: "1",
      dialogVisible: false,
      dialogVisibleT: false,
      tableData: [
        {
          sn: "GW7700000001",
          ip: "10.10.83.173",
          port: "5000",
          version: "1.0",
        },
        {
          sn: "GW7700000002",
          ip: "10.10.83.170",
          port: "5000",
          version: "1.0",
        },
        {
          sn: "GW7700000003",
          ip: "10.10.83.160",
          port: "5000",
          version: "1.0",
        },
      ],
      tableData2: [
        {
          id: "01A500000000001",
          name: "会议室--1",
          time: "2010-11-11 11:11:11",
        },
      ],
      putForm: {},
      EPDForm: {},
      EPDList: [],
      moduleSelection: [],
    };
  },
  methods: {
    handleClose() {
      this.putForm = {};
      this.EPDForm = {};
      this.dialogVisible = false;
      this.dialogVisibleT = false;
    },
    openDialog(i) {
      this.dialogVisible = true;
      console.log(i);
    },
    menuSelect(i) {
      console.log(i);
      this.menuActive = i;
    },
    deleteMsg(i) {
      console.log(i);
      // TODO
    },
    addTable() {
      this.dialogVisibleT = true;
    },
  },
};
</script>

<style lang="scss" scoped>
.el-input,
.el-select {
  width: 100%;
}
.el-date-editor.el-input,
.el-date-editor.el-input__inner {
  width: 100%;
}
</style>