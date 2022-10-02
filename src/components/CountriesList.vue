<template>
  <div v-if="countries" class="row">
    <div class="col-5" style="max-height: 90vh; overflow: scroll">
      <div class="list-group">
        <RouterLink
          v-for="(country, index) in countries"
          :key="index"
          v-bind:to="`/country/${country.alpha3code}`"
        >
          <a
            class="list-group-item list-group-item-action active"
            href="/{{country.alpha3Code}}"
          >
            <img
              v-bind:src="`https://flagpedia.net/data/flags/icon/72x54/${country.alpha2Code.toLowerCase()}.png`"
            />
            <p>{{ country.name.common }}</p>
          </a>
        </RouterLink>
      </div>
      <div><RouterView /></div>
    </div>
  </div>
  <div v-else class="col-12"><Spinner /></div>
</template>

<script setup>
import { ref } from "vue";
// import { useRoute } from "vue-router";
import CountryDetails from "./CountryDetails.vue";
import Spinner from "./Spinner.vue";

//data properties
const countries = ref(null);

//methods/functions
const fetchCountries = async () => {
  const response = await fetch(
    "https://ih-countries-api.herokuapp.com/countries"
  );

  const finalResponse = await response.json();

  const sortedCountries = finalResponse.sort((a, b) => {
    return a.name.common.localeCompare(b.name.common);
  });

  countries.value = sortedCountries;
};

fetchCountries();
</script>
