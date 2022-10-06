<template>
  <div class="navContainer">
    <div class="navBar-logo">
      <RouterLink to="/"
        ><img src="../assets/images/ttlogo.svg" alt="tt logo"
      /></RouterLink>
      <RouterView />
    </div>
    <nav>
      <p>
        Nice to see you <span>{{ name }}</span>
      </p>
      <button @click="logOut">LogOut</button>
    </nav>
  </div>
  <hr />
</template>

<script setup>
import { ref, computed } from "vue";
import { useRouter } from "vue-router";
import { RouterLink, RouterView } from "vue-router";
import { useUserStore } from "../stores/user";

//constant to save a variable that will hold the use router method
const userStore = useUserStore();
// constant to save a variable that will get the user from store with a computed function imported from vue
const user = userStore.user;

// constant that calls user email from the useUSerStore
const userEmail = user.email;
// constant that saves the user email and cleans out the @client from the user
const name = userEmail.replace(/@.*$/, "");
// async function that calls the signOut method from the useUserStore and pushes the user back to the Auth view.
const redirect = useRouter();
const logOut = async () => {
  try {
    await userStore.signOut();
    redirect.push({ path: "/auth/login" });
  } catch (error) {
    errorMsg.value = `Error: ${error.message}`;
    setTimeout(() => {
      errorMsg.value = null;
    }, 5000);
  }
};
</script>

<style scoped>
img {
  width: 60px;
}
.navContainer {
  display: flex;
  justify-content: space-between;
  align-content: center;
  align-items: center;
  width: 90%;
  margin: 0 50px;
  height: 80px;
}
nav {
  display: flex;
  gap: 200px;
  align-items: center;
}
button {
  padding: 10px 30px;
  background: #72c1c1;
  border: 2px solid #406c6c;
  color: #6c4040;
  border-radius: 10px;
  text-align: center;
  text-decoration: none;
  font-size: 16px;
  font-weight: 600;
  cursor: pointer;
  transition: 0.5s;
  box-shadow: 2px 2px 5px #999;
}
button:hover {
  background-color: #406c6c;
  border: 2px solid #72c1c1;
  color: #72c1c1;
  box-shadow: 2px 2px 5px #999;
}

hr {
  box-shadow: 2px 2px 5px #999;
}
</style>
