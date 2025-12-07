<template>
  <!-- Card 1 -->
  <div class="card mt-2">
    <a :href="project.link" target="_blank">
      <img :src="project.image" alt="..." />
      <h3 class="py=5">{{ project.title }}</h3>
      <p class="py=5">{{ project.description }}</p>
    </a>
  </div>
</template>

<script setup>
import { defineProps } from "vue";

defineProps({
  project: Object,
});
</script>