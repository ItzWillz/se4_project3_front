<script setup>
import Utils from "../config/utils";
import { useRouter } from "vue-router";
import facultyServices from "../services/facultyServices.js";
import Listbox from "primevue/listbox";
import { ref, onMounted } from "vue";
import stuAccServices from "../services/stuaccommodationServices.js";

const router = useRouter();

const user = Utils.getStore("user")
console.log(user)

let facultyId = null;
let students = ref([]);

const selectedStudent = ref();
 const selectedStuAcc = ref();
 const stuAcc = ref([]);

facultyServices.getFacultyIdByUserId(Utils.getStore("user").userId)
.then((response) => {
  facultyId = response.data.id

  facultyServices.getAllStudentsForFaculty(facultyId)
  .then((response) => {
  students.value = response.data
  console.log(students)
}).catch((e) => {
  console.log(e)
});

}).catch((e) => {
  console.log(e)
});

const display = (students) => students.name + " " + students.id;

let studisplay = "";

const chooseStudent =() => {
  if (!selectedStudent.value) {
    console.error('Error: No student is selected.');
    return;
  }
    stuAccServices.getAllForUser(selectedStudent.value)
    .then((response) => {
      console.log(response.data)
      stuAcc.value = response.data;
      studisplay = (stuAcc) => stuAcc.accommodationId + " " + stuAcc.semester;
    })
    .catch((e) => {
        console.log(e);
    //   message.value = e.response.data.message;
    });
};

const test =() => {
  console.log(selectedStudent.value);
};

</script>


<template>
  <v-container >

      <v-toolbar>
      <v-toolbar-title>Welcome, {{user.fName}}! </v-toolbar-title>
    </v-toolbar>

<v-row>
<v-col>
        <h2>Current Students with Accommodations</h2>
        <Listbox v-model="selectedStudent"  :options='students' filter :optionLabel= 'display' optionValue="id" 
        :virtualScrollerOptions="{ itemSize: 38 }" class="w-full md:w-14rem" listStyle="height:450px" />

</v-col>

<v-col>
 
    <div style="margin-top: 0.1rem"> 
      <h2 style="text-align: center;">Actions</h2>
            <div style="margin-left: 150px;">
      <div class="row">
       <button class=test @click="chooseStudent()">Select Student </button>

      </div>
            </div>
      </div>
</v-col>
</v-row>


<v-row>
<v-col>
        <h2>Current Student Accommodations</h2>
        <Listbox v-model="selectedStuAcc"  :options='stuAcc' filter :optionLabel= 'studisplay' optionValue="id" 
        :virtualScrollerOptions="{ itemSize: 38 }" class="w-full md:w-14rem" listStyle="height:450px" />

</v-col>

<v-col>
 
    <div style="margin-top: 0.1rem"> 
      <h2 style="text-align: center;">Actions</h2>
            <div style="margin-left: 150px;">
      <div class="row">
       <button class=test @click="viewAccommodation()">View Accommodation </button>

      </div>
            </div>
      </div>
</v-col>
</v-row>

  </v-container>
</template>