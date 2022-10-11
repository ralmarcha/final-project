<template>
  <div id="homeContainer">
    <NewNav />
    <Date />

    <NewList @childNewGroup="addGroupTodo" />

    <ListItem v-for="(item, index) in groupArray" :key="index" :group="item" />
    <!-- @emitGroup="groupList" -->
    <Footer />
  </div>
</template>

<script setup>
import { ref } from "vue";
import { useTaskStore } from "../stores/task";
import NewNav from "../components/NewNav.vue";
import Date from "../components/Date.vue";
import Footer from "../components/Footer.vue";
import NewList from "../components/NewList.vue";
import ListItem from "../components/ListItem.vue";

const taskStore = useTaskStore();

const groupArray = ref([]);

async function readFromStore() {
  groupArray.value = await taskStore.fetchTasks();
  console.log(groupArray.value[0].group);
  groupArray.value = extractGroups(groupArray);
}
readFromStore();

async function addGroupTodo(task) {
  await taskStore.addGroup(task.group);
  readFromStore();
}
async function groupList(task) {
  const taskId = task.id;
  const groupNew = task.group;
  await taskStore.groupList(groupNew, taskId);
  readFromStore();
}

function extractGroups(extract) {
  let arrayResult = [];
  for (let i = 0; i < extract.value.length; i++) {
    arrayResult[i] = extract.value[i].group;
  }
  console.log(arrayResult);
  arrayResult = [...new Set(arrayResult)];
  console.log(arrayResult);
  return arrayResult;
}
</script>

<style>
* {
  font-family: "Roboto Mono", monospace;
  padding: 0;
  margin: 0;
  background-color: antiquewhite;
}
#homeContainer {
  display: flex;
  flex-direction: column;
  margin: 10px 10px 20px 200px;
}
#tasks {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 20px;
}
.taskTran-enter-active {
  transition: all 0.8s ease-out;
}
.taskTran-leave-active {
  transition: all 0.8s cubic-bezier(1, 0.5, 0.8, 1);
}
.taskTran-enter-from,
.taskTran-leave-to {
  transform: translateX(10px);
  opacity: 0;
}
@media screen and (max-width: 768px) {
  #tasks {
    display: flex;
    flex-direction: column;
  }
  #homeContainer {
    margin: 10px;
  }
}
</style>
