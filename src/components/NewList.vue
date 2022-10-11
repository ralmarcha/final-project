<template>
  <div class="container">
    <img id="taskLogo" src="../assets/images/list.jpg" alt="list image" />
    <h1>Add a New List</h1>
    <div class="flex">
      <input
        id="group"
        v-model="group"
        type="text"
        placeholder="Enter a List Name"
      />
      <button @click.prevent="addNewGroup">Add</button>
    </div>
    <div>
      <h3 v-if="errorNoValue">{{ errorMessage }}</h3>
    </div>
  </div>
</template>

<script setup>
import { ref } from "vue";

const emit = defineEmits(["childNewGroup"]);

let group = ref("");

const errorNoValue = ref(false);

const errorMessage = ref("");

const addNewGroup = () => {
  if (group.value === "") {
    errorNoValue.value = true;
  } else {
    const newGroup = {
      group: group.value,
    };
    emit("childNewGroup", newGroup);
    group.value = "";
  }
};
</script>

<style scoped>
* {
  align-content: center;
  background-color: #fcbe6b;
  width: 100%;
  margin-bottom: 20px;
}
#taskLogo {
  width: 150px;
  margin-top: 10px;
  transform: rotate(-15deg);
}
h1 {
  color: #79351f;
  font-size: 24px;
  font-weight: 400;
  text-align: center;
  margin-bottom: 30px;
}
.container {
  outline: 1px solid #79351f;
  outline-offset: -10px;
  width: 100%;
  margin-top: 20px;
}

input {
  font-size: 16px;
  display: block;
  width: 80%;
  height: 100%;
  padding: 5px 10px;
  background: none;
  background-image: none;
  border: 1px solid #79351f;
  color: #79351f;
  margin-bottom: 30px;
  transition: all 0.4s ease;
}
input:focus,
textarea:focus {
  outline: none;
  background-color: #feecb7;
  opacity: 0.5;
  transition: ease-in-out, width 0.35s ease-in-out;
}
::placeholder {
  color: #79351f;
  font-size: 12px;
  opacity: 0.7;
  font-style: italic;
}
button {
  flex: 1;
  height: 40px;
  max-width: 100px;
  padding: 10px;
  border-radius: 10px;
  border: 1px solid #807d48;
  background-color: #d2864c;
  color: #79351f;
  font-size: 18px;
  font-weight: bold;
  letter-spacing: 1px;
}
button:hover {
  opacity: 0.8;
  -webkit-transform: translateY(-3px);
  -ms-transform: translateY(-3px);
  transform: translateY(-3px);
  box-shadow: 0 6px 6px 0 #5a3d2b;
  transition: all 0.3s ease-in-out;
}
.flex {
  display: flex;
  flex-direction: column;
}
@media screen and (max-width: 768px) {
  .container {
    width: 100%;
    margin: 10px;
  }
  #taskLogo {
    display: none;
  }
  h1 {
    margin-top: 30px;
  }
}
</style>
