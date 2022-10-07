<template>
  <div>
    <p>Today's Date is {{ currentDate() }}</p>
  </div>
</template>

<script setup>
import moment from "moment";

function currentDate() {
  return moment().format("MMM Do YY");
}
</script>

<style scoped>
* {
  padding-top: 60px;
  display: flex;
  justify-content: flex-end;
  background-color: indianred;
  width: 90%;
}
</style>
