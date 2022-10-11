<template>
  <div class="container">
    <div class="taskItem">
      <RouterLink id="group" to="/list"> {{ group }}</RouterLink>
      <!-- <div @click="groupList"></div> -->
    </div>
  </div>
</template>

<script setup>
import { ref } from "vue";
const props = defineProps(["group"]);

// const emit = defineEmits(["emitGroup"]);

// const groupList = () => {
//   emit("emitGroup", props.task);
// };
</script>

<style scoped>
* {
  width: 100%;
  background-color: #bcbb85;
}
.taskItem {
  margin-bottom: 10px;
  display: flex;
  flex-direction: row;
  outline: 1px solid #79351f;
  outline-offset: -5px;
}
.container {
  margin-bottom: 20px;
}
img {
  width: 50px;
}
.taskEdit {
  display: flex;
  flex-direction: row;
  width: 45%;
  padding-right: 10px;
}
.taskEdit:hover {
  cursor: pointer;
}
.errorMsg {
  color: #c42727;
  text-align: center;
}

#desc {
  font-size: 14px;
  height: fit-content;
  margin-top: 10px;
  margin-bottom: 10px;
}
#group {
  height: fit-content;
  margin-top: 10px;
  margin-bottom: 10px;
  text-decoration: none;
}
.taskContainer {
  display: flex;
  flex-direction: row;
  padding-left: 10px;
}
.containerFormEdit {
  display: flex;
  flex-direction: column;
  justify-content: center;
  height: fit-content;
  margin-bottom: 20px;
  background-color: #bcbb85;
  width: 90%;
  align-content: center;
  align-items: center;
  padding: 5px 10px;
}
.title {
  display: flex;
  flex-direction: row;
  justify-content: center;
}
.description {
  display: flex;
  flex-direction: row;
  justify-content: center;
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
  height: 30px;
  max-width: 60px;
  border-radius: 5px;
  padding: 8px;
  border: 1px solid #807d48;
  background-color: #d2864c;
  color: #79351f;
  font-size: 12px;
  letter-spacing: 1px;
  text-align: center;
}

.inputEditTask {
  display: flex;
  flex-direction: column;
  width: 60%;
  height: fit-content;

  color: black;
  font-size: 12px;
  background-color: #bcbb85;
  align-items: center;
  text-align: center;
  /* width: 50%; */
}
#inputEditTask-title {
  padding: 5px 10px;
  margin-bottom: 10px;
  border: 1px solid #79351f;
  font-style: italic;
}
#inputEditTask-desc {
  padding: 5px 10px;
  margin-bottom: 10px;
  border: 1px solid #79351f;
  font-style: italic;
}
label {
  width: 40%;
}

@media screen and (max-width: 768px) {
  .taskItem {
    width: 100%;
    margin: 10px;
  }
}
</style>
