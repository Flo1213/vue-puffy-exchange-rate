<template>
  <header>
    <AppHeading />
  </header>
  <main>
    <InputBoard
      :countriesData="countriesData"
      :inputCountriesData="inputCountriesData"
      :fetchCountriesDone="fetchCountriesDone"
      :fetchCurrencyDataDone="fetchCurrencyDataDone"
      @targetCountryInput="handleTargetCountry"
      @countryDataDelete="handleCountryDelete"
    />
    <AddButton @displayAddBoard="handleDisplayAddBoard" />
  </main>
  <AddCountryBoard
    :transformStyle="transformStyle"
    :countriesData="countriesData"
    @addCountryClick="handleAddCountry"
  />
</template>

<script setup>
import AppHeading from "./components/AppHeading.vue";
import InputBoard from "./components/InputBoard.vue";
import AddCountryBoard from "./components/AddCountryBoard.vue";
import AddButton from "./components/AddButton.vue";

import { onMounted, ref, watch } from "vue";

const inputCountriesData = ref([
  {
    countryName: "Taiwan",
  },

  {
    countryName: "Japan",
  },
]);

const countriesData = ref([]);
const countryCurrencyRate = ref([]);
const fetchCountriesDone = ref(false);
const fetchCurrencyDataDone = ref(false);

const transformStyle = ref("translateX(100%)");

onMounted(() => {
  const saveInputCountriesData = JSON.parse(
    localStorage.getItem("inputCountriesData")
  );

  if (saveInputCountriesData) {
    inputCountriesData.value = saveInputCountriesData;
  }
  fetchCountries();
});

// Fetch countries data
const fetchCountries = async function () {
  try {
    const res = await fetch("https://restcountries.com/v3.1/all");
    const data = await res.json();

    countriesData.value = data.map((country) => ({
      countryName: country.name.common,
      currency: country.currencies,
      flag: country.flags,
    }));

    fetchCountriesDone.value = true;
  } catch (error) {
    console.error(`Error fetching data of countries: ${error} `);
  }
};

// Fetching currency rate data
const fetchCurrencyData = async function (currency) {
  try {
    const res = await fetch(
      `https://v6.exchangerate-api.com/v6/b373a9c11a3542a45f331425/latest/${currency}`
    );
    const data = await res.json();

    fetchCurrencyDataDone.value = true;
    countryCurrencyRate.value = data.conversion_rates;
  } catch (error) {
    console.error(error);
  }
};

// Toggle add country board
const handleDisplayAddBoard = () => {
  if (transformStyle.value === "translateX(100%)") {
    transformStyle.value = "translateX(0%)";
  } else {
    transformStyle.value = "translateX(100%)";
  }
};

// Monitor input target country
const handleTargetCountry = async function (targetCurrency) {
  await fetchCurrencyData(targetCurrency.value);
  matchedCountryRate();
};

// Push conversion rate into input country data
const matchedCountryRate = () => {
  for (const [currency, rate] of Object.entries(countryCurrencyRate.value)) {
    inputCountriesData.value.forEach((country) => {
      if (country.currency === currency) {
        country.conversionRate = Number(rate);
      }
    });
  }
};

// Add new country in inputCountriesData
const handleAddCountry = (addCountry) => {
  inputCountriesData.value.push({ countryName: addCountry.value });
  handleDisplayAddBoard();
  savedTransactionsToLocalStorage();
};

// Delete country data deleted
const handleCountryDelete = function (targetDeleteCountry) {
  const index = inputCountriesData.value.findIndex(
    (country) => country.countryName === targetDeleteCountry.value
  );
  if (index !== -1) {
    inputCountriesData.value.splice(index, 1);
  }

  savedTransactionsToLocalStorage();
};

// Render Country Flag
const filterInputCountriesData = () => {
  countriesData.value.forEach((country) => {
    inputCountriesData.value.forEach((inputCountry) => {
      if (country.countryName === inputCountry.countryName) {
        inputCountry.currency = Object.keys(country.currency).join();
        inputCountry.flag = country.flag.svg;
      }
    });
  });
};

// Save to localStorage
const savedTransactionsToLocalStorage = () => {
  localStorage.setItem(
    "inputCountriesData",
    JSON.stringify(inputCountriesData.value)
  );
};

watch(
  inputCountriesData,
  () => {
    filterInputCountriesData();
  },
  { deep: true, immediate: true }
);
</script>

<style>
header {
  padding: 3.6rem;
}

main {
  height: 75dvh;
  width: 80dvw;
  padding: 2.4rem;
  border-top: 1px solid var(--color-gray-primary);

  display: flex;
  flex-direction: column;
  justify-content: space-between;
}

/* AddCountryBoard Style */
</style>
