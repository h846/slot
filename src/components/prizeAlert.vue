<template>
  <div>
    <v-alert
      :value="alert"
      color="pink"
      dark
      transition="scale-transition"
      text-center
      px-4
    >
      <p class="font-weight-bold">
        おめでとうございます！!
        <span class="h1">{{ prize_type }}</span>
        に当選しました。
      </p>
    </v-alert>
    <v-card class="mx-auto" width="300">
      <v-img class="white--text align-end" :src="photo_url"> </v-img>
      <v-card-title class="font-weight-black">{{ item_name }}</v-card-title>
      <v-spacer></v-spacer>
      <v-divider class="mx-4"></v-divider>
      <v-card-subtitle>
        <div>{{ comment }}</div>
      </v-card-subtitle>
      <v-card-actions>
        <v-btn
          color="primary"
          block
          elevation="2"
          x-large
          href="http://lejnet/"
        >
          LEJ-NETへもどる
        </v-btn>
      </v-card-actions>
    </v-card>
  </div>
</template>
<script>
export default {
  props: {
    alert: { type: Boolean, default: false },
    prize_type: String,
    item_name: String,
    comment: String,
    photo_url: String,
  },
};
</script>