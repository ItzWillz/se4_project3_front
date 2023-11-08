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

 const selectedStuAcc = ref();
 const stuAcc = ref([]);

const display = "";

const retrieveStuAcc = () => {
    // Need to retrieve the id from the params
    stuAccServices.getAllForUser(selectedStudent.value)
    .then((response) => {
      console.log(response.data)
      stuAcc.value = response.data;
      display = (stuAcc) => stuAcc.accommodationId + " " + stuAcc.semester;
    })
    .catch((e) => {
        console.log(e);
    //   message.value = e.response.data.message;
    });
};

const goBack =() => {
  router.push({ name: 'faculty'});
};

const test =() => {
  console.log(selectedStudent.value);
};

</script>


<template>
  <v-container >

<v-row>
<v-col>
        <h2>Current Student Accommodations</h2>
        <Listbox v-model="selectedStuAcc"  :options='stuAcc' filter :optionLabel= 'display' optionValue="id" 
        :virtualScrollerOptions="{ itemSize: 38 }" class="w-full md:w-14rem" listStyle="height:450px" />

</v-col>

<v-col>
 
    <div style="margin-top: 0.1rem"> 
      <h2 style="text-align: center;">Actions</h2>
            <div style="margin-left: 150px;">
      <div class="row">
       <button class=test @click="viewAccommodation()">View Accommodation </button>
       <button class=test @click="goBack()">Back </button>

      </div>
            </div>
      </div>
</v-col>
</v-row>

  </v-container>
</template>