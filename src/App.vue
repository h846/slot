<template>
  <v-app>
    <v-main>
      <v-container fill-height>
        <Login @draw="startDrawing" />
        <v-row justify="center" align-content="center">
          <v-col clos="4">
            <div>
              <img :src="slotImg[0]" />
            </div>
          </v-col>
          <v-col clos="4">
            <div>
              <img :src="slotImg[1]" />
            </div>
          </v-col>
          <v-col clos="4">
            <div>
              <img :src="slotImg[2]" />
            </div>
          </v-col>
        </v-row>
        <v-row>
          <v-col>
            <v-btn>ストップ!!</v-btn>
          </v-col>
        </v-row>
      </v-container>
    </v-main>
  </v-app>
</template>

<script>
import Login from "./components/Login";

export default {
  name: "App",

  components: {
    Login,
  },

  data: () => ({
    intervalID: null,
    userInfo: { id: "", name: "" },
    peopleImg: [
      { id: 1, name: "Logan", src: require("@/assets/img/1-logan.png") },
      { id: 2, name: "Masami", src: require("@/assets/img/2-masami.png") },
      { id: 3, name: "risa", src: require("@/assets/img/3-risa.png") },
      { id: 4, name: "Takashi", src: require("@/assets/img/4-takashi.png") },
      { id: 5, name: "Toru", src: require("@/assets/img/5-toru.png") },
      { id: 6, name: "Masakuni", src: require("@/assets/img/6-masakuni.png") },
      { id: 7, name: "Tomoko", src: require("@/assets/img/7-tomoko.png") },
      { id: 8, name: "Yoko", src: require("@/assets/img/8-yoko.png") },
      { id: 9, name: "Tsutomu", src: require("@/assets/img/9-tsutomu.png") },
      { id: 10, name: "Yumiko", src: require("@/assets/img/10-yumiko.png") },
      { id: 11, name: "Ken", src: require("@/assets/img/11-ken.png") },
      { id: 12, name: "Naoko", src: require("@/assets/img/12-naoko.png") },
      {
        id: 13,
        name: "Yoshitaka",
        src: require("@/assets/img/13-yoshitaka.png"),
      },
      {
        id: 14,
        name: "Omatsuri",
        src: require("@/assets/img/14-omatsuri.png"),
      },
      {
        id: 15,
        name: "Operator",
        src: require("@/assets/img/15-operator.png"),
      },
    ],
    slotImg: [],
  }),

  methods: {
    startDrawing(userInfo) {
      this.userInfo = userInfo;
      this.intervalID = setInterval(
        function () {
          this.$set(this.slotImg, 0, this.getRandPeopleImg());
          this.$set(this.slotImg, 1, this.getRandPeopleImg());
          this.$set(this.slotImg, 2, this.getRandPeopleImg());
        }.bind(this),
        100
      );
    },
    getRandPeopleImg() {
      return this.peopleImg[Math.floor(Math.random() * 15)].src;
    },
    hitDetection() {
      
    },
  },

  created() {
    //ランダムに３名の写真を選んでセットする
    for (let i = 3; i != 0; i--) {
      this.slotImg.push(this.getRandPeopleImg());
    }
    console.log(this.slotImg);
  },
};
</script>
<style lang="stylus" scoped></style>
