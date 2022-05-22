<template>
  <v-container fluid class="my-container pa-16">
    <v-row>
      <v-col>
        <v-btn x-large link to="/" color="#8362F2" class="white--text">
          <v-icon right dark class="mr-3"> mdi-arrow-left </v-icon>
          Back to Homepage
        </v-btn>
        <div class="mt-8">
          <div class="d-flex align-center">
            <main-text :text="mainData.name" />
            <img :src="mainData.flag" alt="flag" width="46" height="30" />
          </div>
          <v-chip-group column>
            <v-chip
              v-for="(tag, idx) in mainData.altSpellings"
              :key="idx"
              color="#8DD4CC"
              class="white--text"
            >
              {{ tag }}
            </v-chip>
          </v-chip-group>
        </div>
      </v-col>
    </v-row>
    <v-row class="pr-8">
      <v-col md="5" lg="5">
        <v-card class="pa-4" outlined radius="5px" height="100%">
          <div class="pos-relative">
            <div>Latlong</div>
            <main-text :text="mainData.latlng.join(', ')" color="my-color" />
            <!-- <div class="globe">
              <v-icon class="icon" x-large>mdi-earth</v-icon>
            </div> -->
          </div>
        </v-card>
      </v-col>
      <v-col md="5" lg="5" height="100%">
        <v-card class="pa-4" outlined radius="5px" height="100%">
          <div class="pos-relative">
            <detail-text title="Capital" :value="mainData.capital" />
            <detail-text title="Region" :value="mainData.region" />
            <detail-text title="Subregion" :value="mainData.subregion" />
          </div>
        </v-card>
      </v-col>
      <v-col md="2" lg="2"></v-col>
    </v-row>
    <v-row class="pr-8">
      <v-col md="5" lg="5">
        <div>Calling Code</div>
        <main-text :text="mainData.callingCodes[0]" color="my-color" />
        <div class="d-flex">
          <v-tooltip bottom>
            <template v-slot:activator="{ on, attrs }">
              <div v-bind="attrs" v-on="on" class="my-color text-decoration-underline">
                {{ callingCode.length }} countries
              </div>
            </template>
            <span>
              <div v-for="(call, idx) in callingCode" :key="idx">
                {{ call.name }}
              </div>
            </span>
          </v-tooltip>
          <div class="ml-2">with this calling code</div>
        </div>
      </v-col>
      <v-col md="5" lg="5" height="100%">
        <div>Currency</div>
        <main-text :text="mainData.currencies[0].code" color="my-color" />
        <div class="d-flex">
          <v-tooltip bottom>
            <template v-slot:activator="{ on, attrs }">
              <div v-bind="attrs" v-on="on" class="my-color text-decoration-underline">
                {{ currencies.length }} countries
              </div>
            </template>
            <span>
              <div v-for="(curr, idx) in currencies" :key="idx">
                {{ curr.name }}
              </div>
            </span>
          </v-tooltip>
          <div class="ml-2">with this currency</div>
        </div>
      </v-col>
      <v-col md="2" lg="2"></v-col>
    </v-row>
  </v-container>
</template>

<script>
import axios from "axios";
import MainText from "../components/MainText.vue";
import DetailText from "../components/DetailText.vue";

export default {
  data: () => ({
    name: null,
    mainData: {},
    currencies: [],
    callingCode: [],
  }),
  components: {
    MainText,
    DetailText,
  },
  async created() {
    this.name = this.$route.params.name;
    await this.fetchMainData();
  },
  methods: {
    async fetchMainData() {
      await axios
        .get(`https://restcountries.com/v2/name/${this.name}?fullText=true`)
        .then((res) => {
          const data = res.data[0];
          this.mainData = data;
          this.fetchCallingCodeData(data.callingCodes[0]);
          this.fetchCurrencyData(data.currencies[0].code);
        })
        // .then(() => console.log('data'))
        .catch((err) => console.log(err));
    },
    async fetchCallingCodeData(callingCodeId) {
      await axios
        .get(`https://restcountries.com/v2/callingcode/${callingCodeId}`)
        .then((res) => {
          this.callingCode = res.data;
        })
        .then(() => console.log("koling", this.callingCode))
        .catch((err) => console.log(err));
    },
    async fetchCurrencyData(currencyId) {
      await axios
        .get(`https://restcountries.com/v2/currency/${currencyId}`)
        .then((res) => {
          console.log("currency", res.data);
          this.currencies = res.data;
        })
        // .then(() => console.log(this.mainData))
        .catch((err) => console.log(err));
    },
  },
};
</script>
