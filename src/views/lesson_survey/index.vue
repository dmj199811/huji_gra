<template>
  <div class="outer_bg1">
    <div class="start_top">
      <el-button type="primary" @click="questionModal(1)">发起单选题</el-button>
      <el-button type="primary" @click="questionModal(2)"> 发起多选题</el-button>
      <el-button type="primary" @click="questionModal(3)">发起简答题</el-button>
      <br><br>
      <span>问卷名称： </span><el-input v-model="questionnaireName" style="width:300px" clearable />
      <el-button style="float:right" type="primary" @click="submitData()">保存问卷</el-button>
    </div>
    <!-- 单选弹窗 -->
    <el-dialog title="发起单选" width="650px" :visible.sync="radioVisible">
      <el-form label-width="60px">
        <el-form-item label="题目">
          <el-input v-model="radioQuestion.question" type="textarea" />
        </el-form-item>
        <el-form-item label="给分">
          <el-input v-model="radioQuestion.score" />
        </el-form-item>
        <template v-for="(item,index) in radioQuestion.ansower">

          <el-form-item :key="index+'1'" :label="'选项'+(index+1)">
            <el-input v-model="radioQuestion.ansower[index]" style="width:350px" />
            <el-button type="primary" @click="addAnsower1(index)">添加</el-button>
            <el-button v-if="index>1" type="danger" @click="deleteAnsower1(index)">删除</el-button>
          </el-form-item>
        </template>
      </el-form>
      <el-button type="info" @click="radioVisible = false">取消</el-button>
      <el-button type="primary" @click="radioAdd">确定</el-button>
    </el-dialog>
    <!-- 多选弹窗 -->
    <el-dialog title="发起多选" width="650px" :visible.sync="checkBoxVisible">
      <el-form label-width="60px">
        <el-form-item label="题目">
          <el-input v-model="checkBoxQuestion.question" type="textarea" />
        </el-form-item>
        <template v-for="(item,index) in checkBoxQuestion.ansower">
          <el-form-item :key="index+'7'" :label="'选项'+(index+1)">
            <el-input v-model="checkBoxQuestion.ansower[index]" style="width:350px" />
            <el-button type="primary" @click="addAnsower2(index)">添加</el-button>
            <el-button v-if="index>1" type="danger" @click="deleteAnsower2(index)">删除</el-button>
          </el-form-item>
        </template>
      </el-form>
      <el-button type="info" @click="checkBoxVisible = false">取消</el-button>
      <el-button type="primary" @click="checkBoxAdd">确定</el-button>
    </el-dialog>
    <!-- 简答弹窗 -->
    <el-dialog title="发起简答" width="650px" :visible.sync="sectionVisible">
      <el-form label-width="60px">
        <el-form-item label="题目">
          <el-input v-model="sectionQuestion.question" type="textarea" />
        </el-form-item>
      </el-form>
      <el-button type="info" @click="sectionVisible = false">取消</el-button>
      <el-button type="primary" @click="sectionAdd">确定</el-button>
    </el-dialog>
    <div class="question_box">
      <!-- 单选区域 -->
      <!-- <div v-for="(item,index) in questionData.radio" :key="index+'2'" class="radio_box">
        <span class="title">单选题{{ index+1 }}</span><br>
        问：{{ item.question }}<br>
        <span v-for="(item1,index1) in item.ansower" :key="index1+'3'" style="margin-right:20px">答案{{ index1+1 }}：{{ item1 }}</span>
      </div>
      <br> -->
      <!-- 多选区域 -->
      <!-- <div v-for="(item,index) in questionData.checkBox" :key="index+'5'" class="radio_box">
        <span class="title">多选题{{ index+1 }}</span><br>
        问：{{ item.question }}<br>
        <span v-for="(item1,index1) in item.ansower" :key="index1+'4'" style="margin-right:20px">答案{{ index1+1 }}：{{ item1 }}</span>
      </div>
      <br> -->
      <!-- 简答区域 -->
      <!-- <div v-for="(item,index) in questionData.section" :key="index+'6'" class="radio_box">
        <span class="title">简答题{{ index+1 }}</span><br>
        问：{{ item.question }}<br>
      </div> -->
      <!-- element 折叠框 -->
      <el-collapse v-show="questionData.length" v-model="activeNames">
        <el-collapse-item v-for="(item,index) in questionData" :key="index+'0'" :title="'问题'+(index+1)+'：'+item.question.substring(0,60)+(item.question.length>40?'...':'')" :name="index">
          <p>当前题目分值为 {{ item.score }}</p>
          <span v-for="(item1,index4) in item.ansower" :key="index4+'3'" style="margin-right:20px">答案{{ index4+1 }}：{{ item1 }}</span>
          <span style="float:right">
            <el-button type="primary" @click="updateData(item,index)">修改</el-button>
            <el-button v-if="index!=0" type="primary" @click="questionMove('toTop',index)">上移</el-button>
            <el-button v-if="index!=questionData.length-1" type="primary" @click="questionMove('toBottom',index)">下移</el-button>
          </span>
        </el-collapse-item>
      </el-collapse>
      <!-- 修改弹窗 -->
      <el-dialog title="修改题目" width="650px" :visible.sync="updateVisible">
        <el-form label-width="60px">
          <el-form-item label="题目">
            <el-input v-model="data1.question" type="textarea" />
          </el-form-item>
          <template v-for="(item,index) in data1.ansower">
            <el-form-item :key="index+'1'" :label="'选项'+(index+1)">
              <el-input v-model="data1.ansower[index]" style="width:350px" />
              <el-button type="primary" @click="addAnsower3(index)">添加</el-button>
              <el-button v-if="index>1" type="danger" @click="deleteAnsower3(index)">删除</el-button>
            </el-form-item>
          </template>
        </el-form>
        <el-button type="info" @click="updateVisible = false">取消</el-button>
        <el-button type="primary" @click="toUpdate">确定</el-button>
      </el-dialog>
    </div>
  </div>
</template>
<script>
import { post_json } from '../../utils/utils.js'

export default {
  data() {
    return {
      activeNames: [],
      questionnaireName: '',
      updateVisible: false,
      radioVisible: false,
      checkBoxVisible: false,
      sectionVisible: false,
      radioQuestion: {
        type: 1,
        score: '',
        question: '',
        ansower: ['', '']
      },
      checkBoxQuestion: {
        type: 2,
        score: '',
        question: '',
        ansower: ['', '']
      },
      sectionQuestion: {
        type: 3,
        score: '',
        question: '',
        ansower: []
      },
      questionData: [],
      index1: '',
      data1: {
        type: '',
        question: ''
      }
    }
  },
  methods: {
    submitData() {
      if (this.questionnaireName === '') {
        this.$message.error('请输入当前问卷名称')
        return
      }
      const arr1 = []
      let score = 0
      this.questionData.map((item, index) => {
        arr1[index] = {}
        arr1[index].order = index + 1
        arr1[index].name = this.questionData[index].question
        arr1[index].type = this.questionData[index].type
        arr1[index].score = this.questionData[index].score
        score += +arr1[index].score
        if (JSON.stringify(item.ansower) !== '[]') {
          arr1[index].options = []
          item.ansower.map((item1, index1) => {
            arr1[index].options[index1] = {}
            arr1[index].options[index1].content = this.questionData[index].ansower[index1]
            arr1[index].options[index1].order = index1 + 1
          })
        }
      })
      if (score !== 100) {
        this.$message.error(`当前题目分值为${score}, 请调整至100`)
        return
      }
      const data = {
        name: this.questionnaireName,
        userId: localStorage.getItem('id'),
        username: localStorage.getItem('username'),
        questions: arr1
      }
      post_json('/api/question/survey', data)
    },
    questionMove(data, index) {
      const obj1 = this.questionData[index]
      if (data === 'toTOP') {
        this.questionData[index] = this.questionData[index - 1]
        this.questionData[index - 1] = obj1
      } else if (data === 'toBottom') {
        this.questionData[index] = this.questionData[index + 1]
        this.questionData[index + 1] = obj1
      }
      this.$forceUpdate()
      console.log(this.questionData)
    },
    toUpdate() {
      this.questionData[this.index1] = this.data1
      this.updateVisible = false
    },
    updateData(data, index) {
      this.data1 = JSON.parse(JSON.stringify(data))
      this.index1 = index
      this.updateVisible = true
    },
    radioAdd() {
      this.questionData.push(this.radioQuestion)
      this.radioVisible = false
    },
    checkBoxAdd() {
      this.questionData.push(this.checkBoxQuestion)
      this.checkBoxVisible = false
    },
    sectionAdd() {
      this.questionData.push(this.sectionQuestion)
      this.sectionVisible = false
    },
    addAnsower1(index) {
      this.radioQuestion.ansower.splice(index + 1, 0, '')
    },
    addAnsower2(index) {
      this.checkBoxQuestion.ansower.splice(index + 1, 0, '')
    },
    addAnsower3(index) {
      this.data1.ansower.splice(index + 1, 0, '')
    },
    deleteAnsower1(index) {
      this.radioQuestion.ansower.splice(index, 1)
    },
    deleteAnsower2(index) {
      this.checkBoxQuestion.ansower.splice(index, 1)
    },
    deleteAnsower3(index) {
      this.data1.ansower.splice(index, 1)
    },
    questionModal(data) {
      switch (data) {
        case 1:
          this.radioQuestion = {
            type: 1,
            question: '',
            ansower: ['', '']
          }
          this.radioVisible = true
          break
        case 2:
          this.checkBoxQuestion = {
            type: 2,
            question: '',
            ansower: ['', '']
          }
          this.checkBoxVisible = true
          break
        case 3:
          this.sectionQuestion = {
            type: 3,
            question: ''
          }
          this.sectionVisible = true
          break
      }
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
    overflow: hidden;
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
