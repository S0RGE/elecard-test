<template>
  <div>
    <v-btn class="ma-3" @click="toggleButton" dark>{{
      toggleButtonText
    }}</v-btn>
    <v-btn class="ma-3" @click="resetData" dark>reset data</v-btn>
    <div class="text-center" v-if="loading">
      <v-progress-circular indeterminate color="primary"></v-progress-circular>
    </div>
    <div v-if="toggleCards">
      <Cards :data="sortedData" @closeCard="closeCard" />
    </div>
    <div v-else>
      <Tree
        :data="data"
        :key="forceRender"
      />
    </div>
  </div>
</template>

<script>
import Cards from "../components/Cards.vue";
import Tree from "../components/Tree.vue";

export default {
  props: ["sort"],
  data() {
    return {
      toggleCards: false,
      data: [],
      dataSort: [],
      toggleButtonText: "Tree to Card",
      loading: false,
      forceRender: 0,
    };
  },
  methods: {
    toggleButton() {
      this.toggleCards = !this.toggleCards;
      if (this.toggleCards) {
        this.toggleButtonText = "Card to Tree";
      } else {
        this.toggleButtonText = "Tree to Card";
      }
    },
    sortMethod(a, b) {
      if (!this.sort) this.sort = "category";
      if (a[this.sort] > b[this.sort]) {
        return 1;
      }
      if (a[this.sort] < b[this.sort]) {
        return -1;
      }
      return 0;
    },
    closeCard(timestamp) {
      if(localStorage.Boro){
        localStorage.Boro += JSON.stringify(this.sortedData.find((el) => el.timestamp === timestamp))
      } else {
        localStorage.Boro =  JSON.stringify(this.sortedData.find((el) => el.timestamp === timestamp))
      }
      this.dataSort = this.sortedData.filter((el) => el.timestamp != timestamp)
    },
    async resetData() {
      await this.$store.dispatch("fetchDataAsync");
      this.dataSort = this.$store.getters.getData;
      localStorage.removeItem("Boro");
    },
  },
  computed: {
    sortedData() {
      let data = this.dataSort.length > 0 ? this.dataSort : this.data;
      return data.sort(this.sortMethod);
    },
    // loading() {
    //   console.log('loading', this.$store.getters.getLoading);
    //   return this.$store.getters.getLoading;
    // },
  },
  components: {
    Cards,
    Tree,
  },
  async mounted() {
    this.loading = true;
    // if (localStorage.Boro) {
    //   this.data = JSON.parse(localStorage.Boro);
    //   this.dataSort = this.dataSort.length > 0 ? this.dataSort : this.data;
    // } else {
    //   await this.$store.dispatch("fetchDataAsync");
    //   this.data = this.$store.getters.getData;
    // }
    await this.$store.dispatch("fetchDataAsync");
    this.data = this.$store.getters.getData;
    this.forceRender += 1;
    this.loading = false;
  },
};
</script>

<style>
</style>