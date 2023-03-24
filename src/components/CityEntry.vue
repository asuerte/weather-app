<template>
  <v-card class="rounded-xl">
    <v-card-item>
      <div class="mt-6 ml-1">
        <div class="d-flex">
          <v-card-subtitle class="text-h5"> {{ newCity.name }}</v-card-subtitle>
          <sup>
            <v-chip
              size="x-small"
              text-color="white"
              color="warning"
              class="ml-1"
              >{{ newCity.sys.country }}</v-chip
            >
          </sup>
        </div>
      </div>
      <div class="mt-4">
        <v-card-title class="text-h2 py-5 font-weight-bold"
          >{{ Math.round(newCity.main.temp) }}<sup>°C</sup></v-card-title
        >
        <figure>
          <v-img :src="newCity.icon" max-height="120" class="mr-12"></v-img>
          <v-card-subtitle>{{
            newCity.weather[0].description.toUpperCase()
          }}</v-card-subtitle>
        </figure>
      </div>
    </v-card-item>
    <v-card-actions>
      <v-spacer></v-spacer>
      <v-btn color="button-color" @click="emitRemoveCity(newCity)">Delete</v-btn>
    </v-card-actions>
  </v-card>
</template>

<script>
export default {
  name: "CityEntry",
  props: { newCity: { type: Object } },
  methods: {
    emitRemoveCity(selectedCity) {
      this.$emit("onCityRemove", selectedCity)
    }
  }
};
</script>
