<template>
  <div id="app">
    <el-container style="height: 500px; border: 1px solid #eee">
      <el-aside width="200px" style="background-color: rgb(238, 241, 246)">
        <h1>{{ user.name }}</h1>
        <p>{{ user.desc }}</p>
      </el-aside>

      <el-container>
        <el-main>
          <h2>My Projects</h2>
          <el-table :data="projects" style="width: 75%; margin: auto">
            <el-table-column prop="name" label="Name"></el-table-column>
            <el-table-column
              prop="description"
              label="Description"
            ></el-table-column>
            <el-table-column prop="http_url_to_repo" label="URL">
              <template slot-scope="scope">
                <a
                  :href="scope.row.http_url_to_repo"
                  style="text-decoration:none; color: #356ca0"
                  >{{ scope.row.http_url_to_repo }}</a
                >
              </template>
            </el-table-column>
          </el-table>
        </el-main>
      </el-container>
    </el-container>
  </div>
</template>

<script>
// import Project from "./components/Project.vue";

export default {
  name: "app",
  components: {},
  data() {
    return {
      user_id: window.location.pathname.substring(1) || 2367780,
      user: {},
      projects: []
    };
  },
  methods: {
    getUserInfo() {
      fetch(`https://gitlab.com/api/v4/users/${this.user_id}`)
        .then(res => res.json())
        .then(data => {
          this.user = {
            name: data.name,
            desc: data.bio,
            username: data.username
          };
        });
    },
    getProjects() {
      fetch(`https://gitlab.com/api/v4/users/${this.user_id}/projects`)
        .then(res => res.json())
        .then(data => {
          this.projects = data;
        });
    }
  },
  mounted() {
    this.getUserInfo();
    this.getProjects();
  }
};
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
</style>
