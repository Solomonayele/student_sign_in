<template>
  <div id="app">

 <NewStudentForm v-on:student-added="newStudentAdded"></NewStudentForm>
  <StudentTable
    v-bind:students="students"
    v-on:student-present="studentArrivedOrLeft"
    v-on:delete-student="studentDeleted">
  </StudentTable>
 <StudentMessage v-bind:student="mostRecentStudent"></StudentMessage>
  </div>
</template>

<script>
import NewStudentForm from './components/NewStudentForm.vue'
import StudentTable from './components/StudentTable.vue'
import StudentMessage from './components/StudentMessage.vue'
export default {
  name: 'app',
  data() {
    return {
      students: [],
      mostRecentStudent: {}
    }
  },
  components: {
    StudentTable,
    NewStudentForm,
    StudentMessage
  },
  methods: {
    updateStudents() {
      this.$student_api.getAllStudents().then( students => {
        this.students = students

      })
          .catch ( () => alert('Unable to fetch student list' ) )


    },
    newStudentAdded(student) {
      this.$student_api.addStudent(student).then( () => {
        this.updateStudents()
      })

          .catch ( err => {
            alert('Error adding student. Star Id must be unique')
          })
    },
    studentArrivedOrLeft(student, present) {
      student.present = present // update present value
      this.$student_api.updateStudents(student).then( () => {
        this.mostRecentStudent = student
        this.updateStudents()
      })
          .catch( () => alert( ' Unable to update student ' ))
    },



  }


}
</script>

<style>

</style>