<template>
  <div class="head">
    <img class="head__img" src="../assets/images/heading-img.webp" alt="" />
    <div class="head-box">
      <h1 class="heading-first">Puffy Exchange Rate</h1>
    </div>
    <img class="head__img" src="../assets/images/heading-img.webp" alt="" />
  </div>
</template>

<script setup></script>

<style scoped>
.head {
  display: flex;
  align-items: center;
  justify-content: center;

  gap: 1.2rem;
}

.heading-first {
  font-size: 3.6rem;
  color: var(--color-heading);
}

.head__img {
  width: 3.6rem;
}

.head__icon {
  font-size: 1.6rem;
}
</style>
