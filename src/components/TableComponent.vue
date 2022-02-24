<template>
    <table class="table table-stripped">
        <thead>
            <tr>
                <th>ID</th>
                <th>Firstname</th>
                <th>Lastname</th>
                <th>Sex</th>
                <th>Age</th>
                <th>Status</th>
                <th>Edit</th>
                <th>Delete</th>
            </tr>
        </thead>
        <tbody>
            <tr v-for="student in students" v-bind:key="student.id">
                <td>{{student.id}}</td>
                <td>{{student.fname}}</td>
                <td>{{student.lname}}</td>
                <td>{{student.sex}}</td>
                <td>{{student.age}}</td>
                <td>{{student.enrolled ? "Enrolled": "Unenrolled"}}</td>
                <td><a href="#" data-bs-toggle="modal" v-bind:data-bs-target="`#updateModal-${student.id}`" v-on:click="fetchStudent(student.id)">Edit</a></td>
                <td><a href="#" v-on:click="$emit('delete-student', student.id)">Delete</a></td>

                <div class="modal fade" v-bind:id="`updateModal-${student.id}`" tabindex="-1" aria-labelledby="exampleModalLabel" aria-hidden="true">
                <div class="modal-dialog">
                    <div class="modal-content">
                    <div class="modal-header">
                        <h5 class="modal-title">Update Student</h5>
                        <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
                    </div>
                    <form v-on:submit="updateStudent">
                    <div class="modal-body pl-3 pr-3">
                            <input type="hidden" name="id" v-bind:value="student.id">
                            <div class="mb-3">
                                <label class="form-label float-start">First name</label>
                                <input type="text" class="form-control" v-model="this.fname" name="fname" >
                            </div>
                            <div class="mb-3">
                                <label class="form-label float-start">Last name</label>
                                <input type="text" class="form-control" v-model="this.lname" name="lname" >
                            </div>
                            <div class="mb-3 ">
                                <label class="form-label float-start">Sex</label>
                                <input type="text" class="form-control" v-model="this.sex" name="sex" >
                            </div>
                            <div class="mb-3">
                                <label class="form-label float-start">Age</label>
                                <input type="number" class="form-control" v-model="this.age" name="age" >
                            </div>
                            <div class="mb-3">
                                <label class="form-label float-start">Status</label>
                                <input type="text" class="form-control" v-model="this.enrolled" name="enrolled">
                            </div>      
                          
                    </div>
                    <div class="modal-footer">
                        <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">Close</button>
                        <button type="submit" class="btn btn-primary">Submit</button>
                    </div>
                    </form>
                    </div>
                </div>
                 </div>
            </tr>
        </tbody>
    </table>
</template>
<script>


export default {

    name: "TableComponent",

    data(){
        return{
            id: "",
            fname: "",
            lname: "",
            sex: "",
            age: "",
            enrolled: false
        }
    },
    props:{
        students:Array
    },
    methods:{

        async fetchStudent(id){
            const res = await fetch(`http://localhost:3000/students/${id}`);

            const data = await res.json();
            //return data

            this.id = data.id
            this.fname = data.fname
            this.lname = data.lname
            this.sex = data.sex
            this.age = data.age
            this.enrolled = data.enrolled
        },

       updateStudent(e){
            e.preventDefault()

            if(!this.fname){
                alert("Please add task")
                return
            }

            const newStudent = {
                fname: this.fname,
                lname: this.lname,
                sex: this.sex,
                age: this.age,
                enrolled: this.enrolled
            }

            console.log(newStudent)

            this.$emit('update-student', {
                student: newStudent,
                id: this.id
            })

            this.fname = ""
            this.lname = ""
            this.sex = ""
            this.age = ""
            this.enrolled = false

        },
    }


}
</script>