<template>
  <!-- Card 1 -->
  <div class="card">
    <img :src="project.image" alt="..." />
    <h3 class="py=5">{{ project.title }}</h3>
    <p class="py=5">{{ project.description }}</p>
  </div>
</template>

<script setup>
import { defineProps } from "vue";

defineProps({
  project: Object,
});
</script>