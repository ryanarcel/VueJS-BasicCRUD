<template>
    <h1>Home</h1>
<!-- Button trigger modal -->
<button type="button" class="btn btn-primary" data-bs-toggle="modal" data-bs-target="#addModal">
  Add Student
</button>

<!-- Modal -->
<div class="modal fade" id="addModal" tabindex="-1" aria-labelledby="exampleModalLabel" aria-hidden="true">
  <div class="modal-dialog">
    <div class="modal-content">
      <div class="modal-header">
        <h5 class="modal-title" id="exampleModalLabel">Modal title</h5>
        <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
      </div>
      <form v-on:submit="insertStudent">
      <div class="modal-body">
            <div class="mb-3">
                <label class="form-label float-start">First name</label>
                <input type="text" class="form-control" name="fname" v-model="fname">
            </div>
            <div class="mb-3">
                <label class="form-label float-start">Last name</label>
                <input type="text" class="form-control" name="lname" v-model="lname">
            </div>
            <div class="mb-3 ">
                <label class="form-label float-start">Sex</label>
                <input type="text" class="form-control" name="sex" v-model="sex">
            </div>
            <div class="mb-3">
                <label class="form-label float-start">Age</label>
                <input type="number" class="form-control" name="age" v-model="age">
            </div>
            <div class="mb-3">
                <label class="form-label float-start">Status</label>
                <input type="text" class="form-control" name="enrolled" v-model="enrolled">
            </div>          
      </div>
      <div class="modal-footer">
        <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">Close</button>
        <button type="submit" class="btn btn-primary">Insert Student</button>
      </div>
      </form>
    </div>
  </div>
</div>
</template>

<script>

export default{

    name: "HomeComponent",

    data(){
        return{
            fname: "",
            lname: "",
            sex: "",
            age: "",
            enrolled: false
        }
    },
   
    methods:{

        insertStudent(e){
             e.preventDefault()
            if(!this.fname){
                alert("Please add task")
                return
            }

            const newStudent = {
                id: Math.floor(Math.random()*1000),
                fname: this.fname,
                lname: this.lname,
                sex: this.sex,
                age: this.age,
                enrolled: this.enrolled
            }

            this.$emit('add-student', newStudent)

            this.fname = ""
            this.lname = ""
            this.sex = ""
            this.age = ""
            this.enrolled = false
        }
        

       
    },
     emits: ['add-student']
}




</script>