<template>
					<div class="col-12 col-md-6 col-lg-4 d-flex" :key="project.id">
						<div class="project-card flex-fill" :id="project.id">
							<img :src="project.image" class="card-img-top img-fluid" alt="E-commerce API Documentation Image">
							<div>
								<h5>{{project.title}}</h5>
								<p>{{project.description}}</p>
							</div>
						</div>
					</div>
</template>

<script setup>
	import {defineProps} from "vue";

	defineProps({
		project : Object
	})
</script>