<template>
  <v-app style="background: #0a1f44">
    <v-main>
      <div class="d-flex align-center" style="height: 100%">
        <v-container>
          <v-row
            ><div class="text-h1 font-weight-bold mx-auto text-white">
              Weather App
            </div></v-row
          >
          <v-row class="mt-16">
            <v-col>
              <CityForm @onAddCity="addCity" :cities="cities"></CityForm>
            </v-col>
          </v-row>
          <v-row>
            <v-col v-for="city in cities" :key="city.id" cols="3">
              <CityEntry
                :cities="cities"
                :newCity="city"
                :apiKey="apiKey"
                @onCityRemove="removeCity"
              ></CityEntry>
            </v-col>
          </v-row>
        </v-container>
      </div>
    </v-main>
  </v-app>
</template>

<script>
import CityForm from "@/components/NewCityForm.vue";
import CityEntry from "@/components/CityEntry.vue";

export default {
  name: "App",
  components: { CityForm, CityEntry },
  data() {
    return {
      cities: [],
      isFormValid: undefined,
    };
  },
  methods: {
    addCity(selectedCity) {
      this.cities.push(selectedCity);
    },
    removeCity({ id }) {
      this.cities = this.cities.filter((city) => city.id !== id);
    },
  },
};
</script>
