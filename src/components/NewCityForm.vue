<template>
  <v-form
    v-model="isFormValid"
    style="width: inherit"
    ref="form"
    @submit.prevent="addCity"
  >
    <div class="d-flex">
      <v-text-field
        v-model="searchValue"
        :rules="cityRules"
        bg-color="#ffffff"
        label="Search for a city"
        prepend-inner-icon="mdi-magnify"
      >
      </v-text-field>
      <v-btn color="button-color" size="x-large" type="submit" class="ml-6"
        >Submit</v-btn
      >
    </div>
  </v-form>
</template>

<script>
export default {
  name: "CityForm",
  props: { cities: { type: Array, default: [] } },
  data() {
    return {
      isFormValid: undefined,
      searchValue: "",
      cityRules: [
        (v) => !!v || "Name is required",
        (v) =>
          (v && !this.isCityDisplayed(v)) || "This city is already displayed",
      ],
      apiKey: "af345b36d0e0c03ceaa405fe773093b1",
    };
  },
  methods: {
    async addCity() {
      if (this.isFormValid && this.searchValue.length > 0) {
        let selectedCity = this.searchValue;
        let isCityDisplayed = this.isCityDisplayed(selectedCity);

        if (!isCityDisplayed) {
          let cityData = await this.getCityData(selectedCity);
          this.$emit("onAddCity", cityData);
          this.resetForm();
        }
      }
    },
    isCityDisplayed(selectedCity) {
      let cityDisplayed = false;
      
      for (var city of this.cities) {
        let cityNameUpper = city.name.toUpperCase();
        let selectedCityUpper = selectedCity.toUpperCase();

        if (cityNameUpper === selectedCityUpper) {
          cityDisplayed = true;
        }
      }

      return cityDisplayed;
    },
    async getCityData(city) {
      let url = `https://api.openweathermap.org/data/2.5/weather?q=${city}&appid=${this.apiKey}&units=metric`;
      let response = await fetch(url);
      let cityData = await response.json();

      let { main, name, sys, weather } = cityData;
      let id = `${Math.round(Math.random() * 1000)}`;
      let icon = `https://openweathermap.org/img/wn/${weather[0].icon}@2x.png`;

      return { id, icon, main, name, sys, weather };
    },
    resetForm() {
      this.$refs.form.reset();
    },
  },
};
</script>
