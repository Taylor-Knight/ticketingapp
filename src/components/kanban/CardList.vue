<template>
  <Form class="" @submit.prevent="newProject">
    <FormGroup type="text" label="New Project" id="title" v-model="form.title" :error="errorBag.title" />
    <button type="submit" id="new_project_button">New Project</button>
  </Form>

  <div class="flex overflow-auto w-full card-list-container space-x-4 p-4 h-[80vh]">
        <Sidebar />
        <KanbanBoard v-for="(item, index) in categories" :key="index" :category="item"  />
  </div>
</template>

<script setup>
import { defineAsyncComponent, watch } from "vue";
import {useProjectStore} from "../../stores/project"
import {storeToRefs} from 'pinia'
import { reactive } from "@vue/reactivity";
import { FormGroup, Form, Button } from "@adiranids/vue3-tailwind";
import { api } from "../../assets/api";


const errorBag = reactive({
  title: "",
  user_id: "",
});

const form = reactive({
  title: "",
  user_id: "",
});

function newProject() {

  if (form.title == "")
  errorBag.title = "Please enter your project name"
  else
  errorBag.title = ""      
  
  form.user_id = parseInt(window.localStorage.user)
  
  api().post('/add_new_project', form).then((response) => {
    if (response.status == 200) {
      window.location.reload()
    }
    else 
    {
      alert("something went wrong")
    }

  }).catch(err =>{
    if(err.response.data)
    {
      alert(err.response.data.detail)
    }

  })
}


const KanbanBoard = defineAsyncComponent(() =>
  import(/*webpackChunkName: "kanbanboard"*/ "../organisms/KanbanBoard.vue")
);

const Sidebar = defineAsyncComponent(() =>
  import(/*webpackChunkName: "sidebar"*/ "../dashboard/Sidebar.vue")
);


const store = useProjectStore()
const {categories} = storeToRefs(store)


</script>

<style scoped>
.card-list-container::-webkit-scrollbar {
  width: 20px;
}

/* Track */
.card-list-container::-webkit-scrollbar-track {
  box-shadow: inset 0 0 5px grey;
  border-radius: 10px;
}

/* Handle */
.card-list-container::-webkit-scrollbar-thumb {
    @apply bg-gray-400/70 rounded-lg
}

/* Handle on hover */
.card-list-container::-webkit-scrollbar-thumb:hover {
  @apply bg-gray-100/10
}

#new_project_button {
  color: black;
    background-color: gray;
    padding: 8px;
    border-radius: 12px;
    font-size: 12px;
}
    
</style>
