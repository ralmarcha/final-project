<template>
  <div class="container">
    <h1>Add a New Task</h1>
    <div>
      <input v-model="title" type="text" placeholder="Task's title" />
      <input
        v-model="description"
        type="text"
        placeholder="Task's description"
      />
      <button @click.prevent="addNewTask">Add</button>
    </div>
    <div>
      <h3 v-if="errorNoValue">{{ errorMessage }}</h3>
    </div>
  </div>
</template>

<script setup>
import { ref } from "vue";
import { supabase } from "../supabase";
import { useTaskStore } from "../stores/task.js";

// constant to save a variable that define the custom event that will be emitted to the homeView
const emit = defineEmits(["childNewTask"]);
// constant to save a variable that holds the value of the title input field of the new task
let title = ref("");
// constant to save a variable that holds the value of the description input field of the new task
let description = ref("");
// constant to save a variable that holds an initial false boolean value for the errorMessage container that is conditionally displayed depending if the input field is empty
const errorNoValue = ref(false);
// const constant to save a variable that holds the value of the error message
const errorMessage = ref("");
// arrow function to call the form holding the task title and task description that uses a conditional to first checks if the task title is empty, if true the error message is displayed through the errorMessage container and sets a timeOut method that hides the error after some time. Else, its emmits a custom event to the home view with the task title and task description; clears the task title and task description input fields.
const addNewTask = () => {
  if (title.value === "") {
    errorNoValue.value = true;
    errorMessage.value = "Title is required.";
    setTimeout(() => {
      errorNoValue.value = false;
    }, 3000);
  } else {
    const newTask = {
      title: title.value,
      description: description.value,
    };

    emit("childNewTask", newTask);
    title.value = "";
    description.value = "";
  }
};
</script>

<style scoped>
* {
  align-content: center;
  background-color: burlywood;
  width: 90%;
}
h1 {
  padding-top: 50px;
}
</style>
