<template>
  <div id="app">
    <el-container style="height: 97vh">
      <el-aside width="25%">
        <div style="height:200px; width:200px; margin:auto; margin-top: 20px">
          <img
            style="border-radius: 50%"
            height="100%"
            :src="user.avatar_url"
            alt="avatar_image"
          />
        </div>
        <h1>{{ user.name }}</h1>
        <p>{{ user.desc }}</p>
        <a
          style="color: #0366d6; font-size:1.2em; text-decoration:none;"
          target="_blank"
          rel="noopener noreferrer"
          :href="'https://gitlab.com/' + user.username"
          >{{ `@${user.username}` }}</a
        >
        <el-row class="social">
          <a v-if="user.website" :href="user.website"><i class="fas fa-link"></i></a>
          <a v-if="user.linkedin" :href="user.linkedin"><i class="fab fa-linkedin"></i></a>
          <a v-if="user.twitter" :href="user.twitter"><i class="fab fa-twitter-square"></i></a>
        </el-row>
      </el-aside>

      <el-container>
        <el-main style="background-color: #fafbfc; border-left: solid 1px #ddd">
          <h2>My Projects</h2>
          <el-row :gutter="20" type="flex">
            <el-col :span="8" v-for="project in projects" :key="project.id">
              <el-card class="box-card" shadow="hover">
                <a class="proj_url" :href="project.web_url"
                  >
                  <div class="proj_title"><i class="fab fa-gitlab"></i>{{ project.name }}</div></a
                >
                <div class="proj_desc">{{ project.description }}</div>
              </el-card>
            </el-col>
          </el-row>
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
            avatar_url: data.avatar_url,
            website: data.website_url,
            linkedin: data.linkedin.match('https') ? data.linkedin : `https://linkedin.com/in/${data.linkedin}`,
            twitter: data.twitter.match('https') ? data.twitter : `https://twitter.com/${data.twitter.slice(1)}`
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
  font-size: 1.6em;
  font-weight: 600;
}
p {
  padding: 0px 30px;
  color: #777;
}
.el-row{
  flex-wrap: wrap
}
.el-col {
  margin-top: 10px;
}
.el-card{
  height: 100%
}
.proj_url {
  text-decoration: none;
  color: #0366d6;
  font-weight: 600;
  text-align: left;
}
.proj_desc{
  font-size: 0.85em;
  line-height: 18px;
  color: #586069;
  margin-top: 7px;
  cursor: default;
  text-align: left;
}
i{
  color:#586069;
  padding-right: 7px
}
.social {
  margin-top: 20px;
  font-size: 1.2em
}
.social i {
  padding: 0 10px;
}
</style>
