<template>
  <div class="outer_bg1">
    <div class="start_top">
      <el-button type="primary" @click="questionModal(1)">发起单选题</el-button>
      <el-button type="primary" @click="questionModal(2)"> 发起多选题</el-button>
      <el-button type="primary" @click="questionModal(3)">发起简答题</el-button>
    </div>
    <el-dialog title="发起题目" width="650px" :visible.sync="diaVisible1">
      <el-form label-width="60px">
        <el-form-item label="题目">
          <el-input v-model="radioQuestion.question" type="textarea" />
        </el-form-item>
        <template v-for="(item,index) in radioQuestion.ansower">
          <el-form-item :key="index+'1'" :label="'选项'+(index+1)">
            <el-input v-model="radioQuestion.ansower[index]" style="width:350px" />
            <el-button type="primary" @click="addAnsower(index)">添加</el-button>
            <el-button v-if="index>1" type="danger" @click="deleteAnsower(index)">删除</el-button>
          </el-form-item>
        </template>

      </el-form>
      <el-button type="primary" @click="questionAdd">确定</el-button>
      <!-- <el-input v-model=""></el-input> -->
    </el-dialog>
    <div class="question_box">
      <div v-for="(item,index) in questionData.radio" :key="index+'2'" class="radio_box">
        <span class="title">单选题{{ index+1 }}</span><br>
        问：{{ item.question }}<br>
        <span v-for="(item1,index1) in item.ansower" :key="index1+'3'" style="margin-right:20px">答案{{ index1+1 }}：{{ item1 }}</span>
      </div>
    </div>
  </div>
</template>
<script>
export default {
  data() {
    return {
      diaVisible1: false,
      radioQuestion: {
        question: '',
        ansower: ['', '', '']
      },
      questionData: {
        radio: [],
        checkBox: [],
        textarea: []
      }
    }
  },
  methods: {
    questionAdd() {
      this.questionData.radio.push(this.radioQuestion)
      this.diaVisible1 = false
    },
    addAnsower(index) {
      this.radioQuestion.ansower.splice(index + 1, 0, '')
    },
    deleteAnsower(index) {
      this.radioQuestion.ansower.splice(index, 1)
    },
    questionModal(data) {
      console.log(1)
      this.diaVisible1 = true
    }
  }
}
</script>
<style>
.outer_bg1{
  background-color: #ededed;
}
  .start_top{
    padding: 30px;
    background:white;
    border-bottom-left-radius: 20px;
    border-bottom-right-radius: 20px;
  }
  .question_box{
    margin-top: 40px;
    padding:40px;
    background: white;
    font-size:16px;
    line-height: 24px;
  }
  .title{
    font-size: 20px;
    line-height: 40px;
  }
</style>
