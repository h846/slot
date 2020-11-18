<template>
  <v-row justify="center">
    <v-dialog v-model="dialog" persistent max-width="600px">
      <v-card>
        <v-card-title>
          <span class="headline">社員番号とお名前を入力してください</span>
        </v-card-title>
        <v-card-text>
          <v-container>
            <v-row>
              <v-col cols="12">
                <v-text-field
                  label="社員番号"
                  v-model="userInfo.id"
                  required
                ></v-text-field>
              </v-col>
              <v-col cols="12">
                <v-text-field
                  label="お名前"
                  v-model="userInfo.name"
                  @keydown.enter="verifyUser"
                  required
                ></v-text-field>
              </v-col>
            </v-row>
          </v-container>
        </v-card-text>
        <v-card-actions class="px-6">
          <v-btn
            color="primary"
            block
            rounded
            elevation="2"
            x-large
            @click="verifyUser"
          >
            <h1>抽選!!</h1>
          </v-btn>
        </v-card-actions>
        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn color="accent" text href="http://lejnet">
            LEJ-NETに戻る
          </v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>

    <v-dialog v-model="subDialog.disp" width="400">
      <v-card>
        <v-card-title style="white-space: pre">
          {{ subDialog.msg }}
        </v-card-title>
        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn
            color="primary"
            v-if="userInfo.name != '' && userInfo.id != ''"
            @click="startDrawing"
          >
            スタート！
          </v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
  </v-row>
</template>
<script>
import axios from "axios";

export default {
  data: () => ({
    dialog: true,
    subDialog: { disp: false, msg: "" },
    userInfo: { id: "", name: "" },
    winners: [],
  }),
  methods: {
    verifyUser() {
      if (this.userInfo.name == "" || this.userInfo.id == "") {
        this.subDialog.msg = "社員番号またはお名前が未入力です";
        this.subDialog.disp = !this.subDialog.disp;
        return;
      } else {
        this.subDialog.msg =
          "以下の内容で間違いありませんか？" +
          "\nID: " +
          this.userInfo.id +
          "\n氏名: " +
          this.userInfo.name;
        this.subDialog.disp = !this.subDialog.disp;
      }
    },
    startDrawing() {
      this.subDialog.disp = false;
      this.dialog = false;

      //すでにプレイ済かチェック
      let isPlayed = this.winners.some(
        (item) => item.winner_id == this.userInfo.id
      );

      if (isPlayed) {
        alert("すでに抽選済です！");
        location.href = "http://lejnet/";
      }

      this.$emit("draw", this.userInfo);
    },
  },
  created() {
    // すでに抽選したユーザーかチェックするために当選者情報を取得
    axios
      .get(
        "http://lejnet/API/accdb?db=API/src/cswk/slot-game.accdb&table=winners"
      )
      .then((res) => {
        this.winners = res.data;
      });
  },
};
</script>
<style lang="stylus" scoped></style>