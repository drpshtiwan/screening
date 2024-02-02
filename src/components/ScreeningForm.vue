<template>
  <form @submit.prevent="analyse">
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
        <label for="smokingInput" class="form-label text-black h6">جگەرەت کێشاوە؟</label>
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
        <div class="form-check form-check-inline mx-2">
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
        <div class="form-check form-check-inline mx-2">
          <label class="form-check-label" :for="'fmhx-'+cancer.value">
            {{ cancer.name }}
          </label>
          <input class="form-check-input" :id="'fmhx-'+cancer.value" v-model="fmhx" type="checkbox"
                 :value="cancer.value">
        </div>
      </template>
    </div>
    <hr class="my-4">

    <div class="text-center mb-3">
      <div class="d-grid">
        <button type="submit" class="btn btn-primary font-weight-bolder">
          زانینی ئەنجام
        </button>
      </div>
    </div>
  </form>

  <!--  <div class="alert alert-warning" role="alert">-->
  <!--    A simple warning alert—check it out!-->
  <!--  </div>-->


  <template v-if="suggestedHospitals.length > 0">
    <div class="alert alert-info" role="alert">
      <ul class="mb-0">
        <template v-for="hospital in hospitals">
          <li v-if="collect(suggestedHospitals).contains(hospital.id)">{{ hospital.name }}</li>
        </template>
      </ul>
    </div>
  </template>

</template>

<script setup>
import {ref} from 'vue';
import collect from 'collect.js';
// Cities
const HAWLER = 'hawler';
const SULAYMANI = 'sulaymani';
const DUHOK = 'duhok';

// Cancers
const BREAST = 'breast';
const LUNG = 'lung';
const PROSTATE = 'prostate';
const CERVICAL = 'cervical';
const COLON = 'colon';
const OTHERS = 'others';
const ALL = 'all';

// Investigations
const SELF_EXAMINATION = 'self-examination';
const CLINICAL_EXAMINATION = 'clinical-examination';
const MAMMOGRAPHY_EVERY_TOW_YEAR = 'mammography-every-2-year';
const MAMMOGRAPHY_ANNUALLY = 'mammography-annually';
const CHEST_XRAY = 'c-xray';
const PSA_OR_DRE = 'psa';


const cities = [
  {name: 'هەولێر', value: HAWLER},
  {name: 'سلێمانی', value: SULAYMANI},
  {name: 'دهۆک', value: DUHOK},
  {name: 'هەڵەبجە', value: SULAYMANI},
]

const cancers = [
  {name: 'شێرپەنجەی مەمک', value: BREAST},
  {name: 'شێرپەنجەی سنگ', value: LUNG},
  {name: 'شێرپەنجەی پڕۆستات', value: PROSTATE},
  {name: 'شێرپەنجەی ملی منداڵدان', value: CERVICAL},
  {name: 'شێرپەنجەی کۆلۆن', value: COLON},
  {name: 'شێرپەنجەی تر', value: OTHERS},
]

const hospitals = [
  {id: 1, name: 'نەخۆشخانەی ڕزگاری لە هەولێر', city: HAWLER, type: ALL},
  {id: 2, name: 'کلینیکی نەخۆشیەکانی مەمک لە نەخۆشخانەی رزگاری', city: HAWLER, type: BREAST},
  {id: 3, name: 'سەنتەری مەمک لە هەولێر', city: HAWLER, type: BREAST},
  {id: 4, name: 'نەخۆشخانەی شار لە سلێمانی', city: SULAYMANI, type: ALL},
  {id: 5, name: 'نەخۆشخانەی ئازادی لە دهۆک', city: DUHOK, type: ALL},
];

const investigations = [
  {name: 'ساڵانە خۆپشکنینی مەمک', value: SELF_EXAMINATION, type: BREAST},
  {name: 'پشکنینی مامۆگرافی هەموو ٢ ساڵ جارێک', value: MAMMOGRAPHY_EVERY_TOW_YEAR, type: BREAST},
  {name: 'ساڵانە پشکنینی مەمک لەلایەن پسپۆڕەوە تاکوو تەمەنی ٤٥ ساڵی', value: CLINICAL_EXAMINATION, type: BREAST},
  {name: 'ساڵانە پشکنینی مامۆگرافی لە تەمەنی ٤٥ بۆ ٦٩ ساڵی', value: MAMMOGRAPHY_ANNUALLY, type: BREAST},
  {name: 'ساڵانە پشکنینی مامۆگرافی لە تەمەنی ٤٥ بۆ ٦٩ ساڵی', value: MAMMOGRAPHY_ANNUALLY, type: LUNG},
];

const city = ref('hawler');
const age = ref();
const gender = ref('male');
const maritalStatus = ref('single');
const smoking = ref(true);
const fmhx = ref([]);
const hx = ref([]);
const cancerTypes = collect(cancers).pluck('value');
const suggestedInvestigations = ref([]);
const suggestedHospitals = ref([1, 2]);


function analyse(e) {
  if (typeof age.value == 'undefined') {
    alert('تکایە تەمەن بنووسە!');
    return false;
  }

  // check the breast cancer

}

</script>