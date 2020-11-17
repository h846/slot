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
      <h1>おめでとうございます！!</h1>
      <h1>{{ prize_type }}</h1>
      <h1>に当選しました!!</h1>
    </v-alert>
    <v-card class="mx-auto" max-width="300">
      <v-img
        class="white--text align-end"
        height="auto"
        width="auto"
        :src="photo_url"
      >
      </v-img>
      <v-card-title class="font-weight-black">{{ item_name }}</v-card-title>
      <v-spacer></v-spacer>
      <v-divider class="mx-4"></v-divider>
      <v-card-subtitle>
        <div>{{ comment }}</div>
      </v-card-subtitle>
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