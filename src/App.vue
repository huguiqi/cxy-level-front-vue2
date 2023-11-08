<template>


    <table>
    <tr>
      <td>姓名</td>
      <td>性别</td>
      <td>生日</td>
      <td>图片</td>
      <td>操作</td>
    </tr>
    <tr v-for="(student,index) in students" :key="index" @click="selectUpdate(student)">
      <td>{{student.name}}</td>
      <td>{{student.sex}}</td>
      <td>{{student.birth}}</td>
      <td>{{student.photoUrl}}</td>
      <td><button @click="deleteRow(student.id)">删除</button></td>
    </tr>
    <tr>
      <td><input  v-model="form.name"/></td>
      <td><input  v-model="form.sex"/></td>
      <td><input  v-model="form.birth"/></td>
      <td><input  v-model="form.photoUrl"/></td> 
      <td><button @click="postData">提交</button></td>
   </tr>
  </table>

  
</template>

<script>

import axios from 'axios';
// import { ref } from 'vue';


const BASE_API="http://localhost:9999"

// const {id,name,birth,sex,photoUrl} = ref([]);

export default {
  data () {
    return {
      dialogFormVisible: false,
      formLabelWidth:'100px',
      students: [],
      form: {
        id: '',
        name: '',
        birth: '',
        sex: '',
        photoUrl: ''
      }
    }
  },
  methods: {
      loadData(){
      axios.get(BASE_API+"/student")
    .then(resp=>{
      // console.log(resp);
      this.students=resp.data
      console.log(resp.data);
    })
      }, postData(){
        axios.post("http://localhost:9999/student",{
          id: this.form.id,
          name: this.form.name,
          birth: Date.parse(this.form.birth),
          sex: this.form.sex,
          photoUrl: this.form.photoUrl
        }).then(resp=>{
            console.log("response:"+ resp)
            this.loadData();
        }).catch((error)=>{
            console.error(error);
        });
      },
      deleteRow(studentId){
        axios.delete(BASE_API+"/student/"+studentId)
          .then(()=>{
              this.loadData();
          }).catch((error)=>{
              console.error('这是打印的错误信息'+error);
          });
      },
      selectUpdate(student){
        this.form.id= student.id;
        this.form.name =student.name;
        this.form.sex =student.sex;
        this.form.birth = student.birth;
        this.form.photoUrl = student.photoUrl;
      }
    },
    mounted: function() {
      this.loadData();
    }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
