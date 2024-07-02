<template>
  <div class="button">
    <button class="btn" @click="toggleDisplay">+</button>
  </div>
</template>

<script setup>
const emit = defineEmits(["displayAddBoard"]);

const toggleDisplay = function () {
  emit("displayAddBoard");
};
</script>

<style scoped>
.btn {
  background-color: rgba(255, 255, 255);
  padding: 1rem 3rem;
  border: none;
  cursor: pointer;
  z-index: 999;
  font-size: 2rem;
  font-weight: 700;
  border-radius: 1rem;

  justify-self: end;
}

.btn:hover {
  transform: rotate(180deg);
}
</style>
