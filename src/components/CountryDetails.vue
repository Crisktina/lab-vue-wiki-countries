<template>
  <div class="col-7" v-if="countryInfo">
    <img
      v-bind:src="`https://restcountries.eu/data/${countryInfo.alpha2Code.toLowerCase()}.svg`"
      alt="country flag"
      style="width: 300px"
    />
    <h1>{{ countryInfo.name.common }}</h1>
    <table class="table">
      <thead></thead>
      <tbody>
        <tr>
          <td style="width: 30%">Capital</td>
          <td v-if="countryInfo.capital.length === 0">
            This country does not have a capital
          </td>
          <td v-else>{{ countryInfo.capital[0] }}</td>
        </tr>
        <tr>
          <td>Area</td>
          <td>{{ countryInfo.area }}.km<sup>2</sup></td>
        </tr>
        <tr>
          <td>Borders</td>
          <td>
            <ul>
              <li
                class="list-group-item d-flex justify-content-between align-items-center"
              >
                <p>Borders:</p>
                <p v-if="countryInfo.borders.length === 0">
                  This country has no borders.
                </p>
                <RouterLink
                  v-else
                  :to="`/country/${border}`"
                  v-for="(border, index) in countryInfo.borders"
                  :key="index"
                >
                  {{ border }}
                </RouterLink>
              </li>
            </ul>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script setup>
import { watch, ref, onMounted, computed } from "vue";
import { useRoute } from "vue-router";

//data properties
const countryInfo = ref(null);

const route = useRoute();

const getCountryByAlphaCode = async () => {
  const alpha3Code = route.params.alpha3Code;

  const response = await fetch(
    `https://ih-countries-api.herokuapp.com/countries/${alpha3Code}`
  );

  const finalResponse = await response.json();

  countryInfo.value = finalResponse;

  return { countryInfo };
};

onMounted(() => {
  getCountryByAlphaCode();
});

const countryCode = computed(() => {
  return route.params.alpha3Code;
});

watch(countryCode, (newCountryCode) => {
  getCountryByAlphaCode();
});
</script>
