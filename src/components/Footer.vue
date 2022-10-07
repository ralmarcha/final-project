<template>
  <footer>
    <img id="logo" src="../assets/images/triptick.svg" alt="triptick" />
    <div class="socialMedia">
      <p>Follow Our Travels!</p>
      <ul>
        <li>
          <a href="https://www.instagram.com/" target="”_blank”"
            ><img src="../assets/images/instagram.png" alt="instagram logo"
          /></a>
        </li>
        <li>
          <a href="https://www.youtube.com/" target="”_blank”"
            ><img src="../assets/images/youtube.png" alt="youtube logo"
          /></a>
        </li>
      </ul>
      <div class="copyright">
        <p>COPYRIGHT</p>
        <img src="../assets/images/COPY.svg" alt="copyright" />
        {{ currentYear() }}
        <p>TRIPTICK. All rights reserved.</p>
      </div>
    </div>
  </footer>
</template>

<script setup>
import moment from "moment";

function currentYear() {
  return moment().format("YYYY");
}
</script>

<style scoped>
footer {
  position: absolute;
  bottom: 0;
  width: 100%;
  height: 100px;
  background-color: brown;
}
</style>
