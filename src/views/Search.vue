<template>
  <v-container
    fluid
    class="my-container d-flex justify-center align-center flex-column"
  >
    <div class="font-weight-medium text-h2">Country</div>
    <div class="font-weight-medium text-h2">{{ searchSelected }}</div>
    <div class="mt-5 custom-width">
      <v-autocomplete
        color="#8362F2"
        outlined
        append-icon="mdi-magnify"
        :items="countryList"
        v-model="searchSelected"
        :search-input.sync="searchKey"
        @update:search-input="searchData"
        @change="redirect"
      ></v-autocomplete>
    </div>
  </v-container>
</template>

<script>
import axios from "axios";

export default {
  data: () => ({
    countryList: [],
    searchKey: null,
    searchSelected: null,
    _timerId: null,
  }),
  methods: {
    searchData() {
      if (this.searchKey !== null) {
        clearTimeout(this._timerId);
        this._timerId = setTimeout(() => this.fetchData(), 500);
      }
    },
    fetchData() {
      axios
        .get(`https://restcountries.com/v2/name/${this.searchKey}`)
        .then((res) => {
          const data = res.data;
          const newData = data.map(el => el.name)
          this.countryList = newData.filter((el, idx) => idx < 5);
        })
        .catch((err) => console.log(err));
    },
    redirect() {
      this.$router.push(`/result/${this.searchSelected}`);
    },
  },
};
</script>

//scoped untuk komponen ini saja
<style>
.my-container {
  height: 100vh;
}
.custom-width {
  width: 50%;
}
</style>