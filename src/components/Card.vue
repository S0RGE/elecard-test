<template>
  <v-card class="ma-3" width="344">
    <div class="rightTopCross">
      <v-icon @click="closeCard">mdi-close</v-icon>
    </div>
    <v-img :src="cardImage" height="200px"> </v-img>
    <v-card-title> {{ card.image.split('-')[0].split('/')[1] + "  " +  card.image.split('-').slice(1, card.image.split('-').length-1).join(' ') }} </v-card-title>
    <v-card-subtitle>Category: {{ card.category}} <br> Date: {{new Date(card.timestamp).toLocaleString().substring(0,10) }}<br>size: {{ (card.filesize/10240).toFixed(2) }}mb</v-card-subtitle>
  </v-card>
</template>

<script>
export default {
  name: "Card",
  props: ["card"],
  data() {
    return {
      cardImage: "http://contest.elecard.ru/frontend_data/" + this.card.image,
    };
  },
  methods: {
    closeCard() {
      this.$emit("closeCard", this.card.timestamp);
    },
  },
};
</script>

<style>
.rightTopCross {
  background: white;
  position: absolute;
  right: 5px;
  top: 5px;
  z-index: 10;
  border-radius: 5px;
}
</style>
