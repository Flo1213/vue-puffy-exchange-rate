<template>
  <div
    class="country__container"
    v-for="country in inputCountriesData"
    :key="country.countryName"
  >
    <div class="country__info">
      <div class="country__flag"><img :src="country.flag" alt="" /></div>
      <div class="country__name">
        {{ country.countryName }}
      </div>
    </div>
    <div class="country__currency">
      <input
        type="text"
        class="country__input"
        inputmode="numeric"
        placeholder="Enter the number"
        @input="handleInput(country.currency, $event)"
        v-model="country.calcInputValue"
      />
      <p class="country__code">{{ country.currency }}</p>

      <ion-icon
        class="country__icon"
        name="trash-outline"
        @click="deleteCountryData(country.countryName)"
      ></ion-icon>
    </div>
  </div>
</template>

<script setup>
import { defineProps, watch, ref, nextTick } from "vue";
const emit = defineEmits(["targetCountryInput", "countryDataDelete"]);

const targetCurrency = ref("");
const targetDeleteCountry = ref("");

const props = defineProps({
  countriesData: {
    type: Array,
  },
  inputCountriesData: {
    type: Array,
  },
  fetchCountriesDone: {
    type: Boolean,
  },
  fetchCurrencyDataDone: {
    type: Boolean,
  },
  checkConversionRateOk: {
    type: Boolean,
  },
});

// Monitor input change
const handleInput = (countryCurrency, event) => {
  targetCurrency.value = countryCurrency;
  const targetValue = Number(event.target.value);

  emit("targetCountryInput", targetCurrency);

  handleCalcConversionRate(countryCurrency, targetValue);
};

// Calculate currency rate and display in the input
const handleCalcConversionRate = (countryCurrency, targetValue) => {
  props.inputCountriesData
    .filter((input) => input.currency !== countryCurrency)
    .forEach((other) => {
      if (other.conversionRate !== undefined && !isNaN(other.conversionRate)) {
        other.calcInputValue = (other.conversionRate * targetValue).toFixed(2);
      }
    });
};

// Click and delete country data
const deleteCountryData = (countryName) => {
  targetDeleteCountry.value = countryName;
  emit("countryDataDelete", targetDeleteCountry);
};

// watch(
//   () => [props.fetchCountriesDone],
//   (newVal) => {
//     if (newVal.length) {
//       filterInputCountriesData();
//     }
//   },
//   { immediate: true }
// );

// Watch inputCountriesData add new country and fetch data again
// watch(
//   props.inputCountriesData,
//   () => {
//     filterInputCountriesData();
//   },
//   { deep: true }
// );
</script>

<style scoped>
.country__container {
  display: flex;
  align-items: center;
  justify-content: space-between;

  padding: 1.6rem 0;
}

.country__info {
  width: 30%;
  font-size: 2.4rem;

  display: flex;
  align-items: center;
  gap: 2.4rem;
}

.country__flag img {
  width: 6rem;
  border-radius: 0.5rem;
  overflow: hidden;

  display: flex;
  align-items: center;
  justify-content: center;
}

.country__currency {
  width: 50%;
  font-size: 2rem;

  display: flex;
  align-items: center;
  justify-content: end;

  gap: 2.4rem;
}

.country__input {
  padding: 0.4rem;
  font-size: 2rem;
  font-family: inherit;
  border: 2px solid var(--color-white-primary);
  border-radius: 1rem;
  text-align: center;
  background-color: var(--background-color-primary);
  color: var(--color-white-primary);

  justify-self: start;
}

.country__code {
  width: 6rem;
}

.country__icon {
  width: 2rem;
  color: var(--color-trash-icon);
  cursor: pointer;
}
</style>
