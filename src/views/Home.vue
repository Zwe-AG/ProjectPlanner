<template>
  <div class="home">
    <FilterNav :current="current" @filterValue="current=$event"></FilterNav>
    <div v-for="project in filterdProjects" :key="project.id">
      <SingalProject :project="project" @delete="deleteProject" @update="updateProject"></SingalProject>
    </div>
  </div>
  <!-- {{current}} -->
</template>

<script>
import FilterNav from '../components/FilterNav'
import SingalProject from "../components/SingalProject";
export default {
  name: "Home",
  components: {
    FilterNav,
    SingalProject,
  },
  data() {
    return {
      projects: [],
      current : "all"
    };
  },
  methods: {
    deleteProject(id) {
      this.projects = this.projects.filter((project) => {
        return project.id != id;
      });
    },
    updateProject(id){
      let findProject= this.projects.find((project)=>{
          return project.id === id;
      });
      findProject.complete = !findProject.complete;
    }
  },
  computed: {
    filterdProjects(){
      if(this.current === 'complete'){
        return this.projects.filter((p)=>{
          return p.complete === true
          // return p.complete
        })
      }
      if(this.current === 'ongoing'){
        return this.projects.filter((p)=>{
          return p.complete === false
          // return !p.complete 
        })
      }
      return this.projects;
    }
  },
  mounted() {
    fetch("http://localhost:3000/projects")
      .then((res) => {
        return res.json();
      })
      .then((data) => {
        // console.log(data);
        this.projects = data;
      })
      .catch((err) => {
        console.log(err);
      });
  },
};
</script>
