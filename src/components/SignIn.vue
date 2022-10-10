<template>
  <div class="container">
    <div class="left">
      <div class="div1">
        <!-- <img class="paint" src="../assets/images/pintura.png" alt="paint" /> -->
        <img class="icon" src="../assets/images/ttlogo.svg" alt="tt logo" />
      </div>
      <div class="div2">
        <img
          class="logo"
          src="../assets/images/triptick.png"
          alt="triptick logo"
        />
        <h2>Your Trip Begins Here!</h2>
      </div>

      <!-- <PersonalRouter :route="route" :buttonText="buttonText" /> -->

      <p v-if="errorMsg" class="errorInput">
        {{ errorMsg }}
      </p>

      <div class="div3">
        <h2>Sign In</h2>
        <form @submit.prevent="signIn">
          <!-- <div class="txt_field"> -->
          <!-- <label class="labelEmail" for="">Email</label> -->

          <input
            class="inputEmail"
            type="email"
            placeholder="E-mail"
            v-model="email"
            id="email"
          />
          <br />
          <!-- </div>
          <div class="txt_field"> -->
          <!-- <label class="labelPassword" for="">Password</label> -->

          <!-- <div class="inputPassword"> -->
          <input
            class="passwordInputType"
            :type="passwordFieldType"
            onpaste="return false"
            placeholder="Password"
            v-model="password"
            id="password"
          />
          <!-- <span class="">
            <EyeIcon
              :class="[passwordFieldIcon]"
              @click.prevent="hidePassword = !hidePassword"
            />
          </span> -->
          <!-- </div> -->
          <!-- </div> -->
          <button class="signin" type="submit">Sign In</button>
          <p class="signUp">
            <span class="">Don’t have an account? </span>
            <PersonalRouter
              id="signUp"
              :route="route"
              :buttonText="buttonText"
            />
          </p>
        </form>
      </div>
    </div>
    <div class="rigth">
      <div class="div4">
        <img
          id="travel"
          src="../assets/images/travel.svg"
          alt="time to travel"
        />
      </div>
      <div class="div5">
        <img src="../assets/images/portadafoto.png" alt="camper" />
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, computed } from "vue";
import PersonalRouter from "./PersonalRouter.vue";
import { supabase } from "../supabase";
import { useRouter } from "vue-router";
import { useUserStore } from "../stores/user";
import { storeToRefs } from "pinia";

// Route Variables
const route = "/auth/sign-up";
const buttonText = "Sign Up";

// Input Fields
const email = ref("");
const password = ref("");

// Error Message
const errorMsg = ref("");

//Show hide password variables
const passwordFieldType = computed(() =>
  hidePassword.value ? "password" : "text"
);
const hidePassword = ref(true);

// Router to push user once SignedIn to the HomeView
const redirect = useRouter();

// Arrow function to Signin user to supaBase
const signIn = async () => {
  try {
    // calls the user store and send the users info to backend to logIn
    await useUserStore().signIn(email.value, password.value);
    // redirects user to the homeView
    redirect.push({ path: "/" });
  } catch (error) {
    // displays error message
    // errorMsg.value = `Error: ${error.message}`;
    errorMsg.value = "E-mail & Password required";
    // hides error message
    setTimeout(() => {
      errorMsg.value = null;
    }, 5000);
  }
};
</script>
test
<style scoped>
.container {
  max-width: 1100px;
  /* height: 100vh; */
  display: flex;
  flex-direction: row;
  width: 100%;
  align-items: stretch;
}
.left {
  display: flex;
  flex-direction: column;
  width: 60%;
  align-items: flex-start;
  margin-left: 80px;
}
.rigth {
  display: flex;
  flex-direction: column;
  width: 40%;
}
.icon {
  width: 100px;
}

#travel {
  width: 200px;
  padding-left: 100px;
  margin-top: 60px;
}
.div5 {
  padding: 0;
  margin: 0;
  left: 0px;
  top: 0px;
}
.div2 {
  display: flex;
  flex-direction: column;
}

.logo {
  width: 350px;
}
/*
.errorInput {
  border: 1.5px solid #f24452;
  box-shadow: 0px 2px 13px -10px #f24452;
} */
.div3 {
  box-sizing: border-box;
  position: relative;
  display: block;
  margin-bottom: 80px;
  width: 350px;
  height: 300px;
  border-radius: 5px;
  overflow: hidden;
  z-index: 1;
}
.div3 h2 {
  text-align: center;
  padding: 10px;
  font-weight: lighter;
  text-transform: uppercase;
  color: #5a3d2b;
}
input {
  display: block;
  height: 50px;
  width: 80%;
  margin: 0 auto;
  border: none;
}
input::placeholder {
  -webkit-transform: translateY(0px);
  transform: translateY(0px);
  -webkit-transition: 0.5s;
  transition: 0.5s;
}

input:hover,
input:focus,
input:active:focus {
  color: #cf4e3e;
  outline: none;
  border-bottom: 1px solid #cf4e3e;
}
input:active:focus::placeholder {
  color: #cf4e3e;
  -webkit-transform: translateY(-20px);
  transform: translateY(-20px);
}

.passwordInputType,
.inputEmail {
  position: relative;
  z-index: 1;
  border-bottom: 1px solid rgba(0, 0, 0, 0.1);
  padding-left: 20px;
  font-weight: lighter;
  -webkit-transition: 0.5s;
  transition: 0.5s;
}
.signUp {
  margin-top: 20px;
  text-decoration: none;
  display: inline-block;
  color: #5a3d2b;

  -webkit-transition: 0.5s;
  transition: 0.5s;
}
#signUp {
  color: #5a3d2b;
  font-weight: 600;
}
button {
  margin-top: 20px;
  align-items: center;
  border: none;
  background: #807d48;
  cursor: pointer;
  border-radius: 3px;
  padding: 10px;
  width: 100px;
  color: #5a3d2d;
  box-shadow: 0 3px 6px 0 rgba(0, 0, 0, 0.2);
}

button:hover {
  -webkit-transform: translateY(-3px);
  -ms-transform: translateY(-3px);
  transform: translateY(-3px);
  box-shadow: 0 6px 6px 0 rgba(0, 0, 0, 0.2);
}
</style>
