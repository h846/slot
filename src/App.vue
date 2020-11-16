<template>
  <v-app>
    <v-main>
      <v-container fill-height>
        <Login @draw="startDrawing" />
        <v-row justify="center" align-content="center">
          <v-col v-if="prizeMsg != null">
            {{ prizeMsg }}
          </v-col>
        </v-row>
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
            <v-btn @click="hitDetection">ストップ!!</v-btn>
          </v-col>
        </v-row>
      </v-container>
    </v-main>
  </v-app>
</template>

<script>
import axios from "axios";
import Login from "./components/Login";

export default {
  name: "App",

  components: {
    Login,
  },

  data: () => ({
    intervalID: null,
    prizeInfo: null,
    prizeMsg: null,
    userInfo: { id: "", name: "" },
    peopleImg: [
      { id: 1, name: "Logan", src: require("@/assets/img/1-logan.png") },
      { id: 2, name: "risa", src: require("@/assets/img/3-risa.png") },
      { id: 3, name: "Masami", src: require("@/assets/img/2-masami.png") },
      { id: 4, name: "Masami2", src: require("@/assets/img/2-masami.png") },
      { id: 5, name: "Takashi", src: require("@/assets/img/4-takashi.png") },
      { id: 6, name: "Toru", src: require("@/assets/img/5-toru.png") },
      { id: 7, name: "Masakuni", src: require("@/assets/img/6-masakuni.png") },
      { id: 8, name: "Tomoko", src: require("@/assets/img/7-tomoko.png") },
      {
        id: 9,
        name: "Yoshitaka",
        src: require("@/assets/img/13-yoshitaka.png"),
      },
      { id: 10, name: "Yoko", src: require("@/assets/img/8-yoko.png") },
      { id: 11, name: "Tsutomu", src: require("@/assets/img/9-tsutomu.png") },
      { id: 12, name: "Yumiko", src: require("@/assets/img/10-yumiko.png") },
      { id: 13, name: "Ken", src: require("@/assets/img/11-ken.png") },
      { id: 14, name: "Naoko", src: require("@/assets/img/12-naoko.png") },
      {
        id: 15,
        name: "Operator",
        src: require("@/assets/img/15-operator.png"),
      },
      {
        id: 16,
        name: "Omatsuri",
        src: require("@/assets/img/14-omatsuri.png"),
      },
    ],
    slotImg: [],
  }),

  methods: {
    startDrawing(userInfo) {
      this.userInfo = userInfo;
      //slot-dbから賞品情報を取得
      axios
        .get(
          "http://lejnet/API/accdb?db=API/src/cswk/slot-game.accdb&table=prize"
        )
        .then((res) => (this.prizeInfo = res.data));
      //100ミリ秒ごとに画像を変える
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
      //在庫チェック
      let isOutOfStock = this.prizeInfo.every((val) => val.quantity == 0);
      console.log(isOutOfStock);
      if (isOutOfStock) {
        clearInterval(this.intervalID);
        this.prizeMsg = "在庫切れ！HRかISに連絡してください。";
        return;
      }

      let random40 = Math.floor(Math.random() * 40); // 0~39
      let id = null;
      let prize = null;

      if (random40 < 14) {
        prize = this.prizeInfo[random40];
        id = random40;
      } else if (random40 >= 14 && random40 < 24) {
        prize = this.prizeInfo[14];
        id = 14;
      } else {
        prize = this.prizeInfo[15];
        id = 15;
      }

      if (prize.quantity > 0) {
        clearInterval(this.intervalID);
        this.$set(this.slotImg, 0, this.peopleImg[id].src);
        this.$set(this.slotImg, 1, this.peopleImg[id].src);
        this.$set(this.slotImg, 2, this.peopleImg[id].src);

        this.prizeMsg = `おめでとうございます!!
        ${this.prizeInfo[id].prize_type}
        に当選しました！！
        `;
        console.log(prize);
      } else {
        this.hitDetection();
      }
      //賞品在庫数更新
      let sql =
        "UPDATE prize SET quantity = quantity - 1 WHERE ID = " +
        (parseInt(id) + 1);
      console.log("SQL= " + sql);

      axios
        .post("http://lejnet/API/accdb", {
          db: "API/src/cswk/slot-game.accdb",
          sql: sql,
        })
        .then((res) => {
          //当選者記録
          //ちゃんとカラムも指定してINSERTする
          sql = `INSERT INTO winners VALUES('${this.userInfo.name}', '${this.userInfo.id}', '${prize.prize_type}')`;
          console.log(sql, res);
          axios.post("http://lejnet/API/accdb", {
            db: "API/src/cswk/slot-game.accdb",
            sql: sql,
          });
        })
        .error((e) => console.log(e));
    },
  },
};
</script>
<style lang="stylus" scoped></style>
