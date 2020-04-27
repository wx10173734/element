<template>
  <div>
   <el-row>
       <el-row>
           <el-col :span="24">
               <h1 class="title">填写个人信息</h1>
           </el-col>
       </el-row>
       <el-row>
           <!-- 左侧 -->
           <el-col :span="12">
               <el-form :rules="userRules" ref="ruleForm" :model="ruleForm" label-width="100px">
                   <el-row>
                       <el-col :span="10">
                           <el-form-item label="姓名" prop="inputName">
                               <el-input v-model="ruleForm.inputName"></el-input>
                           </el-form-item>
                       </el-col>
                   </el-row>
                   <el-row>
                       <el-col :span="22">
                           <fieldset>
                               <legend>健康信息</legend>
                               <el-col :span="12">
                                   <el-form-item label="身高" prop="inputHeight">
                                       <el-input v-model="ruleForm.inputHeight"></el-input>
                                   </el-form-item>
                               </el-col>
                               <el-col :span="12">
                                    <el-form-item label="体重" prop="inputWeight">
                                        <el-input v-model="ruleForm.inputWeight"></el-input>
                                    </el-form-item>
                               </el-col>
                           </fieldset>
                       </el-col>
                   </el-row>
                   <el-row>
                       <el-col :span="10">
                           <el-form-item label="性别" prop="userSex">
                           <el-radio-group v-model="ruleForm.userSex">
                               <el-radio label="男"></el-radio>
                               <el-radio label="女"></el-radio>
                           </el-radio-group>
                           </el-form-item>
                       </el-col>
                   </el-row>
                   <el-row>
                       <el-col>
                        <el-form-item label="喜爱的运动" prop="userHobby">
                           <el-checkbox-group v-model="ruleForm.userHobby">
                               <el-checkbox label="篮球" name="type"></el-checkbox>
                                 <el-checkbox label="足球" name="type"></el-checkbox>
                                   <el-checkbox label="羽毛球" name="type"></el-checkbox>
                                     <el-checkbox label="跑步" name="type"></el-checkbox>
                           </el-checkbox-group>
                        </el-form-item>
                       </el-col>
                   </el-row>
                   <el-row>
                       <el-col>
                           <el-form-item label="地址" prop="userAddress">
                               <el-select v-model="ruleForm.userAddress" filterable placeholder="请选择">
                                   <el-option-group v-for="group in options"
                                   :key="group.label" :label="group.label">
                                   <el-option v-for="item in group.options"
                                   :key="item.value"
                                   :label="item.label"
                                   :value="item.value">
                                   </el-option>
                                   </el-option-group>
                               </el-select>
                           </el-form-item>
                       </el-col>
                   </el-row>
                   <el-row>
                       <el-col>
                           <el-form-item label="个人简介" prop="userDesc">
                               <el-input rows="5" type="textarea" v-model="ruleForm.userDesc"></el-input>
                           </el-form-item>
                       </el-col>
                   </el-row>
                   <el-row>
                     <el-col :span="24">
                        <el-form-item>
                          <el-button type="primary" @click="submitForm('ruleForm')">提交</el-button>
                          <el-button @click="resetForm('ruleForm')">重置</el-button>
                        </el-form-item>
                     </el-col>
                   </el-row>
               </el-form>
           </el-col>
           <!-- 右侧 -->
           <el-col :span="12">
               <el-row>
                   <el-col>
                       姓名:{{ruleForm.inputName}}
                   </el-col>
               </el-row>
               <el-row>
                   <el-col>
                       身高:{{ruleForm.inputHeight}}
                   </el-col>
                   <el-col>
                       体重:{{ruleForm.inputWeight}}
                   </el-col>
               </el-row>
               <el-row>
                   性别:{{ ruleForm.userSex}}
               </el-row>
               <el-row>
                   喜爱的运动:{{ruleForm.userHobby.length === 0 ? '' : ruleForm.userHobby}}
               </el-row>
               <el-row>
                   地址:{{ruleForm.userAddress}}
               </el-row>
               <el-row>
                   个人简介:<span v-text="ruleForm.userDesc"></span>
               </el-row>
           </el-col>
       </el-row>
   </el-row>
  </div>
</template>

<script>
import { Button } from 'element-ui'
export default {
  name: 'Form',
  components:{
      'el-button' : Button
  },
  data(){
      return{
          ruleForm:{
              inputName:'',
              inputHeight:'',
              inputWeight:'',
              userSex: '',
              userHobby:[],
              userAddress:'',
              userDesc:'',
          },
          options:[
              {
                  label:'热门城市',
                  options:[{
                      value:'Shanghai',
                      label:'上海'
                  },{
                       value:'Beijing',
                      label:'北京'
                  }]
              },
              {
                  label:'城市名',
                  options:[{
                       value:'Chengdu',
                      label:'成都'
                  },{
                      value:'Shenzhen',
                      label:'深圳'
                  }]
              }
          ],
        userRules:{
          inputName:[
              {required:true, message:'请输入姓名',trigger:'blur'},
              {min:1,max:5,message: '长度在1-5个字符之间',trigger: 'blur' }
            ],
          inputHeight:[
            {required:true, message:'请输入身高',trigger:'blur'},
          ],
          inputWeight:[
            {required:true, message:'请输入体重',trigger:'blur'},
          ],
          userSex:[
            {required:true, message:'请输入性别',trigger:'change'},
          ],
          userHobby:[
            {type:'array',required:true, message:'请至少选择一个兴趣爱好',trigger:'change'},
          ],
          userAddress: [
            {required:true, message:'请选择地址',trigger:'change'},
          ],
          userDesc: [
            {required:true, message:'请输入简介',trigger:'blur'},
          ]

        }

      }
  },
  methods:{
    submitForm(formName){
      this.$refs[formName].validate((valid) => {
          if(valid){
            alert('提交成功!')
          }else{
            alert('请检查输入内容是否合法!')
          }
      })
    },
    resetForm(formName){
      this.$refs[formName].resetFields();
    }

  }
}
</script>

<style>
.title{
    text-align: center;
}
</style>
