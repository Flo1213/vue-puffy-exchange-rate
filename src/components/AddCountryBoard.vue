<template>
  <div class="add" :style="{ transform: transformStyle }">
    <div class="add__box">
      <input
        class="add__input"
        type="text"
        @input="filterSearchCondition"
        @blur="clearInput"
        placeholder="Enter country name"
      />

      <ul class="add__list">
        <li
          class="add__item"
          v-for="query in searchQuery"
          @click="clickAddCountry"
        >
          {{ query.countryName }}
        </li>
      </ul>
    </div>
  </div>
</template>

<script setup>
import { defineProps, ref } from "vue";
const emit = defineEmits(["addCountryClick"]);

const searchQuery = ref([]);
const addCountry = ref("");

const props = defineProps({
  transformStyle: {
    type: String,
    default: "translateX(100%)",
  },
  countriesData: {
    type: Array,
  },
});

const filterSearchCondition = (event) => {
  const inputValue = event.target.value;
  searchQuery.value = props.countriesData.filter((country) =>
    country.countryName.toLowerCase().trim().includes(inputValue)
  );
};

const clickAddCountry = (event) => {
  addCountry.value = event.target.textContent;

  emit("addCountryClick", addCountry);
};

const clearInput = (event) => {
  if (event.type === "blur") {
    event.target.value = "";
  }
};
</script>

<style>
.add {
  position: fixed;
  inset: 0 0 0 50%;

  background-color: rgba(30, 30, 30, 0.8);
  backdrop-filter: blur(0.5rem);
  height: 100vh;

  display: block;
  transition: all 0.3s;

  font-size: 2rem;
}

.add__box {
  height: 100%;

  padding: 2rem;

  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}

.add__input {
  width: 50%;
  height: 2rem;
  padding: 1.5rem 1rem;
  border-radius: 10px;
  font-family: inherit;
  font-weight: 500;

  text-align: center;
  font-size: 1.6rem;
}

.add__list {
  width: 100%;
  height: 80%;
  padding: 0 2rem;

  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;

  overflow-y: auto;
}

.add__item {
  padding: 1rem;
  cursor: pointer;
  font-size: 2rem;
}

.add__item:hover {
  background-color: var(--color-white-primary);
  color: var(--color-background-primary);
}
</style>
