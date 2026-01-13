<template>	
	        <div class="container-fluid">
				<div class="row m-4 p-4 mb-0 g-4">
					<ProjectCard
						v-for="project in projects"
						:project = "project"
					/>
				</div>            	
            </div>
</template>

<script setup>
	import ProjectCard from "./ProjectCard.vue";
	import projects from "../data/projects.json";
	
</script>