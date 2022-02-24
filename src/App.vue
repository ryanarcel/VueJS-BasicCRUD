<template>
  <div class="container">
    <HomeComponent v-on:add-student="insertStudent"/>
    <TableComponent
    v-bind:students="students"
    v-on:update-student="updateStudent"
    v-on:delete-student="deleteStudent"/>
  </div>
</template>

<script>
import HomeComponent from "./components/HomeComponent.vue";
import TableComponent from "./components/TableComponent.vue";

export default {
  name: "App",
  components: {
    HomeComponent,
    TableComponent,
  },  
  data() {
    return {
      students: [],
    };
  },
  methods: {

    async insertStudent(student){
    
      const res = await fetch(`http://localhost:3000/students`,{
        method: 'POST',
        headers:{
          'Content-type': 'application/json',
        },
        body: JSON.stringify(student)
      })

      const data = await res.json()

      this.students = [...this.students, data]

    //  $('.modal').modal('hide');

    },

    async updateStudent({student, id}){

      console.log(id)

      const res = await fetch(`http://localhost:3000/students/${id}`,{
        method: 'PUT',
        headers:{
          'Content-type': 'application/json',
        },
        body: JSON.stringify(student)
      })

      const data = await res.json()

      this.students = [...this.students, data]

      location.reload()

    },

    async fetchStudents() {
      const res = await fetch("http://localhost:3000/students");

      const data = await res.json();
      return data;
    },

    async fetchStudent(id) {
      const res = await fetch(`http://localhost:3000/students/${id}`);

      const data = await res.json();
      return data;
    },

    async deleteStudent(id){
      if(confirm('Are you sure')){
        const res = await fetch(`http://localhost:3000/students/${id}`,{
        method: 'DELETE',
        })
        res.status == 200 ?
        this.students = this.students.filter((student) => student.id !== id): //filters tasks[], returns only data that ids != the param id
        alert('Error deleting task')
      }
    }
  },
  async created() {
    this.students = await this.fetchStudents();
  },
};
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
