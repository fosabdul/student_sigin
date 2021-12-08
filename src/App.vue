<template>
 <div id='app'>
   <New-Student-Form v-on:student-added="newStudentAdded"></New-Student-Form>
    <Student-Table
      v-bind:students="students" v-on:student-arrived-or-left="studentArrivedOrLeft"
      v-on:delete-student="studentDeleted">
    </Student-Table>
    <StudentMessage v-bind:student="mostRecentStudent"></StudentMessage> 
    <!-- name of the prop = student 
    name of the data = mostRecentStudent -->


 </div>
</template>

<script>
import NewStudentForm from './components/NewStudentForm.vue'
import StudentMessage from './components/StudentMessage.vue'
import StudentTable from './components/StudentTable.vue'


export default {
  name: 'app',
  
    components: {
          NewStudentForm,
          StudentMessage,
          StudentTable,

    },
  
  data(){
    return{
      students: [],
      mostRecentStudent: {}  // to modify StudentArrivedorLeft method
    }
  
},

  methods: {
    newStudentAdded(student){
      this.students.push(student)
      this.students.sort(function(s1, s2){
        return s1.name.toLowCase() < s2.name.toLowCase() ? -1 : 1
      })
    },
    studentArrivedOrLeft(student, present){
    // find student in array of students
    //update present attribute

      let updateStudent = this.students.find( function(s){ // it will find the matching student or the first matching one / s = every student in the array
      if (s.name === student.name && s.startID === student.startID){
          return true
      }
    })

    if(updateStudent){ 
      updateStudent.present = present
      this.mostRecentStudent =updateStudent // this will tell msg to studentMessage the most recent student
    }
  },
  studentDeleted(student){
    // fileter returns a new array of all students for whom the function returns true
    this.students = this.students.filter(function (s){
      // this is the student to update
      if (s != student){
        return true
       
      }
      // 

    })
    // to clear the welcome msg after the student is deleted 
    this.mostRecentStudent = {}


  }
}

}

</script>

<style> @import "https://cdn.jsdelivr.net/npm/bootstrap@5.1.3/dist/css/bootstrap.min.css" 




</style>
