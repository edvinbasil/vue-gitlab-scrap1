<template>
  <div id="app">
    <el-container style="height: 97vh">
      <el-aside width="25%" style="background-color: #f5f5f5">
        <div style="height:200px; width:200px; margin:auto; margin-top: 20px">
          <img style="border-radius: 50%" height="100%" :src="user.avatar_url" alt="avatar_image">
        </div>
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
            username: data.username,
            avatar_url: data.avatar_url
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
h1 {
  font-size: 1.6em
}
p{
  padding: 0px 30px;
  color: #777
}
</style>
