<template>
  <div class="taskContainer">
    <p>{{ task.title }}</p>
    <p>{{ task.description }}</p>
  </div>
  <!-- CHANGE STATUS -->
  <div class="taskEdit">
    <span v-if="task.is_complete" @click="toggleStatus" class="done">Done</span>
    <span v-if="!task.is_complete" @click="toggleStatus" class="notDone"
      >Not Done</span
    >
    <!-- EDIT TASK -->
    <span class="edit" @click="handleForm">Edit</span>
    <!-- DELETE TASK -->
    <span class="delete" @click="removeTask">delete</span>
  </div>
  <!-- EDIT FORM -->
  <form v-if="showForm" @submit.prevent="editTask" class="containerFormEdit">
    <div v-if="errorHandle" class="errormsg">{{ error }}</div>
    <input
      type="text"
      placeholder="Edit Title"
      v-model="editTitle"
      class="inputEditTask"
    />
    <input
      type="text"
      placeholder="Edit Description"
      v-model="editDescription"
      class="inputEditTask"
    />
    <button type="submit">Save</button>
  </form>
</template>

<script setup>
// 1. ref() or reactive() can be used here to store the following, think if you want to store them either individually or like an object, up to you.
import { ref } from "vue";

const props = defineProps(["task"]);

//2. Data properties need here are the following: a boolean to store a false**Important variable, a string to store an error, a string to store the value of the task that the user can edit, an initial false boolean to hide the inputFIeld used to edit the new task detail or details[this is in reference of the task title and the task description].

const errorHandle = ref(false);
const error = ref("");
const showForm = ref(false);
const editTitle = ref("");
const editDescription = ref("");

//3. Store the custom emit events that will be used to call the functions of the homeView for editing, deleting and toggling the status[completed, not complted] of the taskItem.

const emit = defineEmits(["emitRemove", "emitStatus", "emitEdit"]);

//5. Function to handle the edit dialogue where the inputField is displayed and the string used to store the value of the inputField will be used here to save the value as a prop on this function.
const handleForm = () => {
  showForm.value = !showForm.value;
  editTitle.value = props.task.title;
  editDescription.value = props.task.description;
};

//6. Function to emmit a custom event emit() that takes 2 parameters a name for the custom event and the value that will be send via the prop to the parent component. This function can control the toggle completion of the task on the homeview.

const toggleStatus = () => {
  emit("emitStatus", props.task);
};

//4. Function to handle the error with the data properties used to control the error and the the boolean controlling the boolean empty variable.
//7. Function to edit the task information that you decided that the user can edit. This function's body takes in a conditional that first checks if the value of the task [either title and description or just title] is empty which if true it runs the function used to handle the error on hint4. Else, the conditional sets the first mentioned boolean data property on hint2 back to its inital boolean value to hide the error message and repeat the same for the data property mentioned 4th on hint2; a constant that stores an object that holds the oldValue from the prop item and the value of the task coming from the data property mentioned 3rd on hint2; a custom event emit() that takes 2 parameters a name for the custom event and the value from the object used on this part of the conditional and lastly this part of the conditional sets the value of input field to an empty string to clear it from the ui.
const editTask = () => {
  if (editTitle.value === "" || editDescription.value === "") {
    error.value = "Required inputs";
    errorHandle.value = true;
  } else {
    error.value = "";
    const editValues = {
      oldValue: props.task,
      newTitle: editTitle.value,
      newDescription: editDescription.value,
    };
    emit("emitEdit", editValues);

    setTimeout(() => {
      showForm.value = false;
    }, 1000);
  }
  setTimeout(() => {
    error.value = "";
  }, 3000);
};

//8. Function to emmit a custom event emit() that takes 2 parameters a name for the custom event and the value that will be send via the prop to the parent component. This function can control the removal of  the task on the homeview.

const removeTask = () => {
  emit("emitRemove", props.task);
};
</script>

<style scoped>
* {
  background-color: grey;
  width: 90%;
}
</style>
