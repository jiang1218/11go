<template>
  <div class="dashboard-container">
    <div class="app-container">
      <!-- 第一行 -->
      <el-row>
        <el-col :span="6">
          学科：
          <el-select v-model="searchForm.subjectID">
            <el-option
              v-for="item in subjectList"
              :key="item.value"
              :label="item.label"
              :value="item.value"
            ></el-option>
          </el-select>
        </el-col>
        <el-col :span="6">
          难度：
          <el-select v-model="searchForm.difficulty">
            <el-option
              v-for="item in difficultyList"
              :key="item.value"
              :label="item.label"
              :value="item.value"
            ></el-option>
          </el-select>
        </el-col>
        <el-col :span="6">
          试题类型：
          <el-select v-model="searchForm.questionType">
            <el-option
              v-for="item in questionTypeList"
              :key="item.value"
              :label="item.label"
              :value="item.value"
            ></el-option>
          </el-select>
        </el-col>
        <el-col :span="6">
          标签：
          <el-select v-model="searchForm.tags">
            <el-option
              v-for="item in tagsList"
              :key="item.value"
              :label="item.label"
              :value="item.value"
            ></el-option>
          </el-select>
        </el-col>
      </el-row>

      <!-- 第二行 -->
      <el-row>
        <el-col :span="6">
          城市:
          <el-select style="width:107px;" v-model="searchForm.province" class="cityStyle">
            <el-option
              v-for="(item, index) in provincesList"
              :key="index"
              :value="item"
              :label="item"
            ></el-option>
          </el-select>
          <el-select style="width:107px;" v-model="searchForm.citys">
            <el-option v-for="(item, index) in citysList" :key="index" :value="item" :label="item"></el-option>
          </el-select>
        </el-col>
        <el-col :span="6">
          关键字:
          <el-input placeholder="请输入题目编号/题干" v-model="searchForm.keyword" style="width: 210px"></el-input>
        </el-col>
        <el-col :span="6">
          题目备注：
          <el-input placeholder="请输入" v-model="searchForm.remarks" style="width: 210px"></el-input>
        </el-col>
        <el-col :span="6">
          公司简称：
          <el-input placeholder="请输入" v-model="searchForm.shortName" style="width: 185px"></el-input>
        </el-col>
      </el-row>

      <!-- 第三行 -->
      <el-row>
        <el-col :span="6">
          方向：
          <el-select v-model="searchForm.direction">
            <el-option
              v-for="(item, index) in directionList"
              :key="index"
              :value="index"
              :label="item"
            ></el-option>
          </el-select>
        </el-col>
        <el-col :span="6">
          录入人：
          <el-select v-model="searchForm.creatorID">
            <el-option value="0" label="XXXieChenHao"></el-option>
          </el-select>
        </el-col>
        <el-col :span="6">
          二级目录：
          <el-input v-model="searchForm.catalogID" placeholder="请输入二级目录" style="width: 210px"></el-input>
        </el-col>
        <el-col class="col_btn" :span="6" style="float:right">
          <el-button @click="clearForm">清除</el-button>
          <el-button type="primary" @click="search">搜索</el-button>
        </el-col>
      </el-row>

      <!-- 数据展示区 -->
      <el-table :data="tableData" stripe style="width: 100%" highlight-current-row>
        <el-table-column prop="id" label="序号" width="50"></el-table-column>
        <el-table-column prop="number" label="试题编号"></el-table-column>
        <el-table-column prop="subjectID" label="学科" width="100"></el-table-column>
        <el-table-column prop="questionType" label="题型"></el-table-column>
        <el-table-column prop="question" label="题干"></el-table-column>
        <el-table-column prop="addDate" label="录入时间"></el-table-column>
        <el-table-column prop="difficulty" label="难度"></el-table-column>
        <el-table-column prop="tags" label="试题标签"></el-table-column>
        <el-table-column prop="creator" label="录入人" ></el-table-column>
        <el-table-column label="操作">
          <el-button type="text">预览</el-button>
          <el-button type="text">修改</el-button>
          <el-button type="text">删除</el-button>
          <el-button type="text">加入精选</el-button>
        </el-table-column>
      </el-table>

      <!-- 分页功能 -->
      <el-card>
        <el-pagination
      @size-change="handleSizeChange"
      @current-change="handleCurrentChange"
      :current-page="searchForm.page"
      :page-sizes="[10, 20, 30, 40]"
      :page-size="10"
      :total="total"
      layout="sizes, prev, pager, next, jumper"
      >
    </el-pagination>
      </el-card>
    </div>
  </div>
</template>

<script>
import { simple as subSimple } from '@/api/hmmm/subjects' // 引入学科
import { simple as tagsSimple } from '@/api/hmmm/tags' // 标签
import { provinces, citys } from '@/api/hmmm/citys'
import {
  difficulty as difficultyList,
  questionType as questionTypeList,
  direction as directionList
} from '@/api/hmmm/constants' // 引入常量

import { choice } from '@/api/hmmm/questions' // 引入题库 API

export default {
  name: 'QuestionsList',
  data() {
    return {
      subjectList: [],
      tagsList: [],
      provincesList: [],
      citysList: [],

      difficultyList, // 难度下拉菜单
      questionTypeList, // 试题类型下拉菜单
      directionList, // 方向
      tableData: null,
      total: 0,

      searchForm: {
        subjectID: '', // 学科
        difficulty: '', // 难度
        questionType: '', // 试题类型
        tags: '', // 标签名称
        province: '', // 企业所在地省份
        city: '', // 企业所在城市
        keyword: '', // 关键字
        remarks: '', // 题目备注
        shortName: '', // 企业简称
        direction: '', // 方向
        creatorID: '', // 录入人
        catalogID: '', // 目录
        page: 1,
        pagesize: 10
      }
    }
  },
  created() {
    this.getSubjectList()
    this.getTagslist()
    this.getProvincesList()
    this.search()
  },
  watch: {
    'searchForm.province'(newV) {
      if (newV) {
      this.getCityList(newV)
      }
    }
  },
  methods: {
    async getSubjectList() {
      let result = await subSimple()
      this.subjectList = result.data
    },
    async getTagslist() {
      let result = await tagsSimple()
      this.tagsList = result.data
    },
    async getProvincesList() {
      let result = await provinces()
      this.provincesList = result
    },
    async getCityList(pname) {
      let result = await citys(pname)
      this.citysList = result
    },
    clearForm() {
      this.$confirm('此操作将会清空所有选择', '是否继续', '提示', {
        type: 'warning'
      }).then(() => {
        this.searchForm = {
          subjectID: '', // 学科
          difficulty: '', // 难度
          questionType: '', // 试题类型
          tags: '', // 标签名称
          province: '', // 企业所在地省份
          city: '', // 企业所在城市
          keyword: '', // 关键字
          remarks: '', // 题目备注
          shortName: '', // 企业简称
          direction: '', // 方向
          creatorID: '', // 录入人
          catalogID: '', // 目录
          page: 1,
          pagesize: 10
        }
      })
    },
    async search() {
      let pro = await choice(this.searchForm)
      console.log(pro)
      this.tableData = pro.data.items
      this.total = pro.data.counts
    },
    handleSizeChange(val) {
      // 分页大小
      this.searchForm.pagesize = val
      this.search()
    },
    handleCurrentChange (val) {
      // 当前选择页面
      this.searchForm.page = val
      this.search()
    }
  }
}
</script>

<style scoped>
.el-row {
  margin: 10px 20px;
}

.cityStyle {
  margin-left: 10px;
}
.el-table {
  margin-top: 40px;
}

.el-card {
  text-align: center;
}
</style>
