<template>
  <div class="dashboard-container">
    <div class="app-container">
      <el-form :model="addForm">
        <el-form-item>
          学科：
          <el-select></el-select>
        </el-form-item>
        <el-form-item>
          目录：
          <el-select></el-select>
        </el-form-item>
        <el-form-item>
          企业：
          <el-select></el-select>
        </el-form-item>
        <el-form-item>
          城市：
          <el-select></el-select>
        </el-form-item>
        <el-form-item>
          方向：
          <el-select></el-select>
        </el-form-item>
        <el-form-item>
          题型：
          <el-radio-group v-model="isright">
            <el-radio label="单选"></el-radio>
            <el-radio label="多选"></el-radio>
            <el-radio label="简答"></el-radio>
          </el-radio-group>
        </el-form-item>
        <el-form-item>
          难度：
          <el-radio-group v-model="isright">
            <el-radio label="简单"></el-radio>
            <el-radio label="一般"></el-radio>
            <el-radio label="困难"></el-radio>
          </el-radio-group>
        </el-form-item>

        <el-form-item>
          题干：
          <el-input type="textarea"></el-input>
        </el-form-item>

        <!-- 单选输入 -->
        <el-form-item>
          <el-radio-group v-model="singleRight">
            A:<el-radio>
              <el-input></el-input>
            </el-radio>
            <br>
            B:<el-radio>
              <el-input></el-input>
            </el-radio>
            <br>
            C:<el-radio>
              <el-input></el-input>
            </el-radio>
            <br>
            D:<el-radio>
              <el-input></el-input>
            </el-radio>
            <br>
          </el-radio-group>
        </el-form-item>
        <!-- 多选输入 -->
        <el-form-item>
          <el-checkbox-group v-model="singleRight">
            A:<el-checkbox>
              <el-input></el-input>
            </el-checkbox>
            <br>
            B:<el-checkbox>
              <el-input></el-input>
            </el-checkbox>
            <br>
            C:<el-checkbox>
              <el-input></el-input>
            </el-checkbox>
            <br>
            D:<el-checkbox>
              <el-input></el-input>
            </el-checkbox>
            <br>
          </el-checkbox-group>
        </el-form-item>
        <!-- 简答题输入 -->
        <el-item-form>
          <el-input type="textarea"></el-input>
        </el-item-form>
        <el-item-form>
          解析视频:
          <el-input width="300"></el-input>
        </el-item-form>
      </el-form>
    </div>
  </div>
</template>

<script>
export default {
  name: 'QuestionsNew',
  data() {
    return {
      isright: false,
      singleRight: false,
      addForm: {}
    }
  }
}
</script>

<style scoped>
.el-select {
  width: 70%;
}
</style>
