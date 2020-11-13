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
                  label="社員番号またはID (任意) "
                  v-model="userInfo.id"
                ></v-text-field>
              </v-col>
              <v-col cols="12">
                <v-text-field
                  label="*お名前"
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
            v-if="userInfo.name == ''"
            color="green darken-1"
            outlined
            text
            @click="subDialog.disp = false"
          >
            OK
          </v-btn>
          <v-btn
            color="primary"
            v-if="userInfo.name != ''"
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
export default {
  data: () => ({
    dialog: true,
    subDialog: { disp: false, msg: "" },
    userInfo: { id: "", name: "" },
  }),
  methods: {
    verifyUser() {
      if (this.userInfo.name == "") {
        this.subDialog.msg = "お名前を入力してください！";
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
      this.$emit("draw", this.userInfo);
    },
  },
};
</script>
<style lang="stylus" scoped></style>