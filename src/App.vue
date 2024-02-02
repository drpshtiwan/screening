<script setup>
import ScreeningForm from  './components/ScreeningForm.vue';
const year = new Date();

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
    <div class="bg-primary py-4 py-lg-5 px-2 text-center">
      <h2 class="text-white">پشکنینی پێشوەختەی شێرپەنجە باوەکان</h2>
    </div>
  </header>
  <main class="container ">
    <div class="card card-custom">
      <div class="card-body p-3 p-md-5">
        <ScreeningForm />
      </div>
    </div>



    <hr class="bg-secondary mt-5 " />
    <div class="d-flex text-center text-lg-start flex-column flex-lg-row justify-content-between px-3 mt-2">
      <p>©{{year.getFullYear()}} وەزارەتی تەندروستی حکومەتی هەرێمی کوردستان, گشت مافەکانی پارێزراوە. </p>
      <p>Developed by <a href="https://github.com/drpshtiwan" target="_blank">Dr. Pshtiwan</a></p>
    </div>



  </main>
</template>

<style scoped>
</style>
