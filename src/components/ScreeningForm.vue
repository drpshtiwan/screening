<script setup>
import {ref} from 'vue';

const cities = [
  {name: 'هەولێر', value: 'hawler'},
  {name: 'سلێمانی', value: 'sulaymani'},
  {name: 'دهۆک', value: 'duhok'},
  {name: 'کەرکووک', value: 'kirkuk'},
]

const cancers = [
  {name: 'شێرپەنجی مەمک', value: 'breast'},
  {name: 'شێرپەنجی سنگ', value: 'lung'},
  {name: 'شێرپەنجی پڕۆستات', value: 'prostate'},
  {name: 'شێرپەنجی ملی رەحم', value: 'cervical'},
  {name: 'شێرپەنجی کۆلۆن', value: 'colon'},
  {name: 'شێرپەنجی تر', value: 'others'},
]

const city = ref('hawler');
const age = ref();
const gender = ref('male');
const maritalStatus = ref('single');
const smoking = ref(true);
const fmhx = ref([]);
const hx = ref([]);
</script>
<template>
  <h3 class="mb-3">پێویستە چ پشکنینێک بکەم؟</h3>
  <p class="text-secondary ">ئەم فۆڕمەی خوارەوە پڕبکەوە بۆ زانینی ئەو پشکنینە پێشوەختانەی پێویستە ئەنجامی بدەیت بۆ
    شێرپەنجە باوەکان.</p>
  <hr class="my-4">
  <div class="row">
    <div class="col-12 col-md-6 col-lg-4 col-xl-3 mb-3">
      <label for="cityInput" class="form-label text-black h6">شوێنی نیشتەجێبوون</label>
      <select v-model="city" id="cityInput" class="form-select">
        <option v-for="city in cities" :value="city.value">{{ city.name }}</option>
      </select>
    </div>

    <div class="col-12 col-md-6 col-lg-4 col-xl-2 mb-3">
      <label for="ageInput" class="form-label text-black h6">تەمەن</label>
      <input v-model="age" required type="number" class="form-control " id="ageInput" placeholder="تەمەن بە ژمارە">
    </div>
    <div class="col-12 col-md-6 col-lg-4 col-xl-2 mb-3">
      <label for="genderInput" class="form-label text-black h6">ڕەگەز</label>
      <select v-model="gender" id="genderInput" class="form-select">
        <option value="male">نێر</option>
        <option value="female">مێ</option>
      </select>
    </div>

    <div class="col-12 col-md-6 col-lg-4 col-xl-3 mb-3">
      <label for="maritalStatusInput" class="form-label text-black h6">دۆخی خێزانی</label>
      <select v-model="maritalStatus" id="maritalStatusInput" class="form-select">
        <option value="single">سەڵت</option>
        <option value="married">خێزاندار</option>
      </select>
    </div>

    <div class="col-12 col-md-6 col-lg-4 col-xl-2 mb-3">
      <label for="smokingInput" class="form-label text-black h6">جگەرە کێشیت؟</label>
      <select v-model="smoking" id="smokingInput" class="form-select">
        <option value="true">بەڵی</option>
        <option value="false">نەخێر</option>
      </select>
    </div>
  </div>
  <hr class="my-4">
  <div>
    <h5 class="mb-2">هیچ کام لەم شێرپەنجانەت هەیە؟</h5>
    <template v-for="cancer in cancers">
      <div class="form-check form-check-inline">
        <label class="form-check-label" :for="'hx-'+cancer.value">
          {{ cancer.name }}
        </label>
        <input class="form-check-input" :id="'hx-'+cancer.value" v-model="hx" type="checkbox"
               :value="cancer.value">
      </div>
    </template>
  </div>
  <hr class="my-4">
  <div>
    <h5>هیچ کام لەم شێرپەنجانە لە خیزانەکت هەیە؟</h5>
    <p class="text-black-50 mb-2">کەسی یەکەمی خێزان کە دایک، باوک، برا و خوشک دەگرێتەوە.</p>
    <template v-for="cancer in cancers">
      <div class="form-check form-check-inline">
        <label class="form-check-label" :for="'fmhx-'+cancer.value">
          {{ cancer.name }}
        </label>
        <input class="form-check-input" :id="'fmhx-'+cancer.value" v-model="fmhx" type="checkbox"
               :value="cancer.value">
      </div>
    </template>
  </div>
  <hr class="my-4">

  <div class="text-center my-4">
    <div class="d-grid">
      <button type="submit" class="btn btn-primary font-weight-bolder">
        زانینی ئەنجام
      </button>
    </div>
  </div>


</template>