<script setup>
import ScreeningForm from  './components/ScreeningForm.vue';
</script>

<template>
  <header>
    <div class="text-center py-4">
      <div class="logos">
        <img src="./assets/ribbon.png" alt="Cancer Ribbon" class="mx-1 mx-md-3">
        <img src="./assets/moh.png" alt="MOH" class="mx-1 mx-md-3">
        <img src="./assets/krg.png" alt="KRG" class="mx-1 mx-md-3">
      </div>
    </div>
    <div class="bg-primary py-5 px-2 text-center">
      <h2 class="text-white">پشکنینی پێشوەختەی شێرپەنجە باوەکان</h2>
    </div>
  </header>
  <main class="container ">
    <div class="card card-custom">
      <div class="card-body p-3 p-md-5">
        <ScreeningForm />
      </div>
    </div>
  </main>
</template>

<style scoped>
</style>
