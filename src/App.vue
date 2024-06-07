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
    goToPage(url){
      console.log(url);
      this.callApi(url);

    },
    search(){
      const searchUrl = this.base_api_url + this.projects_endpoint + `?search=${this.search_text}`;
      console.log(searchUrl);
      this.callApi(searchUrl);

    },
    callApi(url){
      axios
        .get(url)
        .then(response => {
          console.log(response);
          this.projects = response.data.results;
          console.log(this.projects );
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
        <div class="col" v-for="project in projects.data">
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
            <div class="card-footer">
              <!-- Modal trigger button -->
              <button
                type="button"
                class="btn btn-primary"
                data-bs-toggle="modal"
                :data-bs-target="`#project-${project.id}`"
              >
                View
              </button>
              
              <!-- Modal Body -->
              <!-- if you want to close by clicking outside the modal, delete the last endpoint:data-bs-backdrop and data-bs-keyboard -->
              <div
                class="modal fade"
                :id="`project-${project.id}`"
                tabindex="-1"
                data-bs-backdrop="static"
                data-bs-keyboard="false"
                
                role="dialog"
                :aria-labelledby="`modal-title-${project.id}`"
                aria-hidden="true"
              >
                <div
                  class="modal-dialog modal-dialog-scrollable modal-dialog-centered modal-md"
                  role="document"
                >
                  <div class="modal-content">
                    <div class="modal-header">
                      <h5 class="modal-title" :id="`modal-title-${project.id}`">
                        {{project.title}}
                      </h5>
                      <button
                        type="button"
                        class="btn-close"
                        data-bs-dismiss="modal"
                        aria-label="Close"
                      ></button>
                    </div>
                    <div class="modal-body">
                      <img class="img-fluid w-100" :src="project.cover_image.startsWith('https://') ? project.cover_image : base_api_url + '/storage/' + project.cover_image" alt="">
                    </div>
                    <div class="modal-footer">
                      <button
                        type="button"
                        class="btn btn-secondary"
                        data-bs-dismiss="modal"
                      >
                        Close
                      </button>
                    </div>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>

      <nav aria-label="Page navigation" class="mt-3">
      <ul class="pagination">
        <li class="page-item" :class="{'disabled' : !link.url, 'active':link.active}" v-for="link in projects.links">
          <button class="page-link" :href="link.url" type="button" @click="goToPage(link.url)">
            <span v-html="link.label">
            </span>
          </button>
        </li>
      </ul>
    </nav>
    </div>

    
    

  </section>

</template>

<style >
</style>
