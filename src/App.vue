<script>
import axios from 'axios';
export default{
  name: 'App',
  data(){
    return{
      base_api_url: 'http://127.0.0.1:8000',
      projects_endpoint: '/api/projects',
      projects: null,
      search_text: ''
    }
  },
  methods:{
    search(){
      const searchUrl = this.base_api_url + this.projects_endpoint + `?search=${this.search_text}`;
      console.log(searchUrl)

    },
    callApi(url){
      axios
        .get(url)
        .then(response => {
          console.log(response);
          this.projects = response.data.results;
        })
        .catch(errors => {
          console.error(errors)
        })
    }
  },
  mounted(){
    const url = this.base_api_url + this.projects_endpoint;
    console.log(url);
    this.callApi(url);
  }
}
</script>

<template>
  <div class="p-5 mb-4 bg-light rounded-3">
    <div class="container py-5">
      <h1 class="display-5 fw-bold">Projects</h1>
      <p class="col-md-8 fs-4">
        Read our amazing blog with our projects
      </p>

      <form @submit.prevent="search()">
        <div class="input-group mb-3">
          <input type="search" class="form-control" placeholder="Search ..." v-model="search_text">
          <button class="btn btn-outline-secondary" type="submit">
            <i class="fa fa-search fa-lg fa-fw"></i>
          </button>
        </div>
      </form>
      
    </div>
  </div>


  <section class="projects" v-if="projects">
    <div class="container">
      <div class="row row-cols-1 row-cols-sm-2 row-cols-md-3 g-4">
        <div class="col" v-for="project in projects">
          <div class="card">
            <!-- Manage the null value inside project.cover_image in these two ways-->
            <!-- 
              <template v-if="project.cover_image && project.cover_image.startsWith('https://')">
              <img :src="project.cover_image" alt="Image">
            </template>
            <template v-else-if="project.cover_image && project.cover_image.startsWith('uploads')">
              <img :src="base_api_url + '/storage/' + project.cover_image" alt="Image">
            </template> 
          -->
          <div v-if="project.cover_image">
            <img class="card-img-top" 
            :src="project.cover_image.startsWith('https://') ? project.cover_image : base_api_url + '/storage/' + project.cover_image" 
            alt="Image of project">
          </div>
          <div v-else>
            <!-- Random image if there isn't a project image -->
            <img src="https://picsum.photos/400/200" alt="random image">

          </div>


            <div class="card-body">
              {{ project.title }}
            </div>
          </div>
        </div>
      </div>
    </div>

  </section>

</template>

<style >
</style>
