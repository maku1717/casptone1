<template>
  <div class="container text-center">
    <h1 class="text-center-projects" id="projects">My Projects</h1>

    <div
      v-for="(group, index) in chunkedProjects"
      :key="index"
      class="card-deck my-5 justify-content-center"
    >
      <ProjectsCard
        v-for="project in group"
        :key="project.id"
        :project="project"
      />
    </div>
  </div>
</template>

<script setup>
import { computed } from "vue";
import ProjectsCard from "./ProjectsCard.vue";
import projects from "../data/projects.json";

const chunkSize = 3;
const chunkedProjects = computed(() => {
  const chunks = [];
  for (let i = 0; i < projects.length; i += chunkSize) {
    chunks.push(projects.slice(i, i + chunkSize));
  }
  return chunks;
});
</script>