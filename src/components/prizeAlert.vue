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
      <p class="font-weight-bold text-center ma-0">
        おめでとうございます！
        <span class="font-weight-black">{{ prize_type }}</span>
        に当選しました！
      </p>
    </v-alert>
    <v-card>
      <v-img :src="photo_url" contain max-height="200" class="mx-auto"> </v-img>
      <div class="text-body font-weight-black text-center my-3">
        {{ item_name }}
      </div>
      <v-spacer></v-spacer>
      <v-divider class="mx-4"></v-divider>
      <v-card-subtitle>
        <div>{{ comment }}</div>
      </v-card-subtitle>
      <v-card-actions>
        <v-btn
          color="primary"
          den
          block
          elevation="2"
          depressed
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
