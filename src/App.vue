<script setup>
  import Navbar from "./components/Navbar.vue";
  import AboutMe from "./components/AboutMe.vue";
  import Projects from "./components/Projects.vue";
  import Tools from "./components/Tools.vue";
  import Contacts from "./components/Contacts.vue";
  import Footer from "./components/Footer.vue";
</script>

<template>
<div id="outer1">
  <Navbar/>
  <AboutMe/>
</div>
<div id="projects">
  <h2 class="text-center py-1 py-lg-5"><span class="first-text">Completed</span> <span class="second-text">Projects</span></h2>
  <Projects/>
</div>
<div id="tool" class="py-5">
  <Tools/>
</div>
<div id="contact">
  <Contacts/>
  <Footer/>
</div>

</template>

<style scoped>

</style>
