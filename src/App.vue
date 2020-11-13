<template>
  <v-app>
    <v-main>
      <v-container fill-height>
        <Login @draw="setUserInfo" />
        <v-row justify="center" align-content="center">
          <v-col>
            <ul v-for="(img, idx) in peopleImg" :key="idx">
              <li>
                <img :src="img.src" :alt="img.name" />
              </li>
            </ul>
          </v-col>
          <v-col>
            <ul v-for="(img, idx) in peopleImg" :key="idx">
              <li>
                <img :src="img.src" :alt="img.name" />
              </li>
            </ul>
          </v-col>
          <v-col>
            <ul v-for="(img, idx) in peopleImg" :key="idx">
              <li>
                <img :src="img.src" :alt="img.name" />
              </li>
            </ul>
          </v-col>
        </v-row>
      </v-container>
    </v-main>
  </v-app>
</template>

<script>
// import Login from "./components/Login";

export default {
  name: "App",

  components: {
    // Login,
  },

  data: () => ({
    userInfo: { id: "", name: "" },
    peopleImg: [
      { name: "Logan", src: require("@/assets/img/1-logan.png") },
      { name: "Masami", src: require("@/assets/img/2-masami.png") },
      { name: "risa", src: require("@/assets/img/3-risa.png") },
      { name: "Takashi", src: require("@/assets/img/4-takashi.png") },
      { name: "Toru", src: require("@/assets/img/5-toru.png") },
      { name: "Masakuni", src: require("@/assets/img/6-masakuni.png") },
      { name: "Tomoko", src: require("@/assets/img/7-tomoko.png") },
      { name: "Yoko", src: require("@/assets/img/8-yoko.png") },
      { name: "Tsutomu", src: require("@/assets/img/9-tsutomu.png") },
      { name: "Yumiko", src: require("@/assets/img/10-yumiko.png") },
      { name: "Ken", src: require("@/assets/img/11-ken.png") },
      { name: "Naoko", src: require("@/assets/img/12-naoko.png") },
      { name: "Yoshitaka", src: require("@/assets/img/13-yoshitaka.png") },
    ],
  }),

  methods: {
    setUserInfo(userInfo) {
      this.userInfo = userInfo;
    },
  },
};
</script>
<style lang="stylus" scoped>
ul {
  li {
    list-style-type: none;
    text-align: center;
  }
}
</style>
