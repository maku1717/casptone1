<template>
  <div class="container center-content" id="landing">
    <img src="./images/joshua.png" alt="joshua" class="profile-pic" />
    <h4 class="py-1">Welcome to the Matrix, I am Josh ✌🏻</h4>
    <h1
      class="py-2 typing-effect"
      data-text="The Architect: Full Stack Web Developer"
    >
      The Architect: Full Stack Web Developer
    </h1>
    <p class="subtext text-light">
      I bend the digital reality as a Zuitt student, forging paths in web
      development.
    </p>
    <div class="buttons">
      <a href="#contact" class="btn primary">Lets Talk</a>
      <a
        :href="cvUrl"
        target="_blank"
        class="btn primary"
        download="MarkJoshuaAmarilleCV.pdf"
        >Download CV</a
      >
    </div>
  </div>
</template>

<script>
import cvUrl from "/assets/MarkJoshuaAmarilleCV.pdf";

export default {
  data() {
    return {
      cvUrl,
    };
  },
};
</script>
