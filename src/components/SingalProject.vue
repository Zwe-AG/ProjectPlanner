<template>
  <div class="project" :class="{ complete: project.complete }">
    <div class="flexing">
      <div @click="showDetail = !showDetail">
        <h3>{{ project.title }}</h3>
      </div>
      <div>
        <span class="material-icons" @click="deleteProject()"> delete </span>
        <router-link :to="{name:'EditProject',params : {id : project.id }}">
          <span class="material-icons"> edit </span>
        </router-link>
        <span class="material-icons" @click="completeProject"> done </span>
      </div>
    </div>
    <p v-if="showDetail">{{ project.detail }}</p>
  </div>
</template>

<script>
export default {
  props: ["project"],
  data() {
    return {
      showDetail: false,
      api: "http://localhost:3000/projects/",
    };
  },
  methods: {
    deleteProject() {
      fetch(this.api + this.project.id, { method: "DELETE" })
        .then(() => {
          this.$emit("delete", this.project.id);
        })
        .catch((err) => {
          console.log(err);
        });
    },
    completeProject(){
      let updatedComplete = this.api+this.project.id;
      fetch(updatedComplete,{
        method:"PATCH",
        headers :{
          "Content-Type" : "application/json"
        },
        body:JSON.stringify(
          {
            complete : !this.project.complete
          }
        )
        }).then(()=>{
           this.$emit('update',this.project.id);
        }); // repair only one complete using patch & repair more complete using push
    }
  },
};
</script>

<style scoped>
.project {
  width: 500px;
  padding: 20px 10px;
  background-color: #f2f2f2;
  cursor: pointer;
  margin: 10px auto;
  border-radius: 6px;
  user-select: none;
  border-left: 5px solid crimson;
}
h3 {
  color: indigo;
  text-align: start;
}
p {
  text-align: start;
}
.flexing {
  display: flex;
  justify-content: space-between;
  align-items: center;
}
span {
  margin-left: 10px;
  color:black
}
span:hover {
  cursor: pointer;
  color: #777;
}
.complete {
  border-left-color: green;
}
</style>
