<template>
  <h3 class="mb-3">پێویستە چ پشکنینێک بکەم؟</h3>
  <p class="text-secondary ">ئەم فۆڕمەی خوارەوە پڕبکەوە بۆ زانینی ئەو پشکنینە پێشوەختانەی پێویستە ئەنجامی بدەیت بۆ
    شێرپەنجە باوەکان.</p>
  <hr class="my-4">
  <div class="mb-3">
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
          <option value="non-smoker">نەخێر</option>
          <option value="smoker">بەڵی</option>
        </select>
      </div>
    </div>
    <hr class="mb-4">
    <div>
      <h5 class="mb-2">هیچ کام لەم شێرپەنجانەت هەیە؟</h5>
      <div class="px-3">
        <template v-for="cancer in cancers">
          <div class="form-check  d-block d-lg-inline-block mx-2" v-if="checkCancerByGender(cancer.value)">
            <label class="form-check-label" :for="'hx-'+cancer.value">
              {{ cancer.name }}
            </label>
            <input class="form-check-input" :id="'hx-'+cancer.value" v-model="hx" type="checkbox"
                   :value="cancer.value">
          </div>
        </template>
      </div>
    </div>
    <hr class="my-4">
    <div class="mb-4">
      <h5>هیچ کام لەم شێرپەنجانە لە خێزانەکەت هەیە؟</h5>
      <p class="text-black-50 mb-2">کەسی یەکەمی خێزان کە دایک، باوک، برا و خوشک دەگرێتەوە.</p>
      <div class="px-3">
        <template v-for="cancer in cancers">
          <div class="form-check d-block d-lg-inline-block mx-2">
            <label class="form-check-label" :for="'fmhx-'+cancer.value">
              {{ cancer.name }}
            </label>
            <input class="form-check-input" :id="'fmhx-'+cancer.value" v-model="fmhx" type="checkbox"
                   :value="cancer.value">
          </div>
        </template>
      </div>
    </div>
  </div>

  <div class="text-center ">
    <div class="d-grid ">
      <button @click.prevent="analyse" class="btn btn-primary font-weight-bolder">
        زانینی ئەنجام
      </button>
    </div>
  </div>


  <template v-if="suggestedInvestigations.length > 0">
    <hr class="my-4">
    <h4 class="mb-3">پێویستە ئەم پشکنینانەی خوارەوە ئەنجام بدەیت</h4>
    <div class="alert alert-warning" role="alert">
      <ul class="mb-0">
        <template v-for="investigation in investigations">
          <li v-if="collect(suggestedInvestigations).contains(investigation.value)">{{ investigation.name }}</li>
        </template>
      </ul>
    </div>
  </template>

  <template v-if="suggestedHospitals.length > 0">
    <hr class="my-4">
    <h4 class="mb-3">دەتوانیت سەردانی ئەم نەخۆشخانەی خوارەوە بکەیت بۆ ئەنجامدانی پشکنینەکان</h4>
    <div class="alert alert-info" role="alert">
      <ul class="mb-0">
        <template v-for="hospital in hospitals">
          <li v-if="collect(suggestedHospitals).contains(hospital.value)">{{ hospital.name }}</li>
        </template>
      </ul>
    </div>
  </template>

  <template v-if="suggestedInvestigations.length > 0">
    <hr class="my-4">
    <div class="text-center">
      <h5 class="mb-3">
        ئەنجامەکان پشت بەستن بە گایدلاینی
        <a class="text-decoration-none" href="/cancer-screening-guidelines.pdf" target="_blank">
          پشکنینی پێشوەختەی شێرپەنجە.
        </a>
      </h5>
    </div>

  </template>

</template>
<script setup>
import {ref, watch} from 'vue';
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
const OVARIAN = 'ovarian';
const PANCREATIC = 'pancreatic';
const OTHERS = 'others';
const ALL = 'all';

// Investigations
const SELF_EXAMINATION = 'self-examination';
const CLINICAL_EXAMINATION = 'clinical-examination';
const MAMMOGRAPHY_EVERY_TOW_YEAR = 'mammography-every-2-year';
const MAMMOGRAPHY_ANNUALLY = 'mammography-annually';
const CHEST_XRAY = 'c-xray';
const PSA_OR_DRE = 'psa';
const HPV = 'hpv-dna';
const PAP_SMEAR_EVERY_THREE_YEARS = 'pap-smear-every-three-years';
const PAP_SMEAR_EVERY_FIVE_YEARS = 'pap-smear-every-five-years';
const FIT = 'fit';
const COLONOSCOPY_EVRY_TEN_YEAR = 'colonoscopy-every-ten-year';
const COLONOSCOPY_EVRY_FIVE_YEAR = 'colonoscopy-every-five-year';
const NO_TESTS_REQUIRED = 'no-tests-required';

//HOSPITALS
const RIZGARY = 'rizgary';
const BREAST_CENTER = 'breast-center';
const MATERNITY_HOSPITAL = 'maternity-hospital';
const SHAR = 'shar';
const AZADI = 'azadi';

//options
const MALE = 'male';
const FEMALE = 'female';
const SINGLE = 'single';
const MARRIED = 'married';
const SMOKER = 'smoker';
const NON_SMOKER = 'non-smoker';

const cities = [
  {name: 'هەولێر', value: HAWLER},
  {name: 'سلێمانی', value: SULAYMANI},
  {name: 'دهۆک', value: DUHOK},
  {name: 'هەڵەبجە', value: SULAYMANI},
]
const cancers = [
  {name: 'شێرپەنجەی مەمک', value: BREAST},
  {name: 'شێرپەنجەی سییەکان', value: LUNG},
  {name: 'شێرپەنجەی پڕۆستات', value: PROSTATE},
  {name: 'شێرپەنجەی ملی منداڵدان', value: CERVICAL},
  {name: 'شێرپەنجەی هێلکەدان', value: OVARIAN},
  {name: 'شێرپەنجەی پەنکریاس', value: PANCREATIC},
  {name: 'شێرپەنجەی کۆلۆن', value: COLON},
  {name: 'شێرپەنجەی تر', value: OTHERS},
]
const investigations = [
  {name: 'خۆپشکنینی مەمک ساڵانە بۆ شێرپەنجەی مەمک.', value: SELF_EXAMINATION, type: BREAST},
  {name: 'پشکنینی مامۆگرافی هەموو ٢ ساڵ جارێک بۆ شێرپەنجەی مەمک.', value: MAMMOGRAPHY_EVERY_TOW_YEAR, type: BREAST},
  {name: 'پشکنینی مەمک لەلایەن پسپۆڕەوە ساڵانە بۆ شێرپەنجەی مەمک.', value: CLINICAL_EXAMINATION, type: BREAST},
  {name: 'پشکنینی مامۆگرافی ساڵانە بۆ شێرپەنجەی مەمک.', value: MAMMOGRAPHY_ANNUALLY, type: BREAST},
  {name: 'وێنەی تیشکی سنگ (Chest Xray) و سەردانی پزیشکی پسپۆڕ بۆ شێرپەنجەی سییەکان.', value: CHEST_XRAY, type: LUNG},
  {
    name: 'پشکنینی خوێنی PSA یاخود پشکنینی پرۆستات لەلایەن پزیشکی پسپۆڕەوە هەموو ١ تا ٢ ساڵ جارێک بۆ شێرپەنجەی پڕۆستات.',
    value: PSA_OR_DRE,
    type: PROSTATE
  },
  {name: 'پشکنینی HPV DNA هەموو ٥ تا ١٠ ساڵ جارێک بۆ شێرپەنجەی ملی منداڵدان.', value: HPV, type: CERVICAL},
  {
    name: 'پشکنینی خانەزانی (Pap Smear) هەموو ٣ ساڵ جارێک بۆ شێرپەنجەی ملی منداڵدان.',
    value: PAP_SMEAR_EVERY_THREE_YEARS,
    type: CERVICAL
  },
  {
    name: 'پشکنینی خانەزانی (Pap Smear) هەموو ٥ ساڵ جارێک بۆ شێرپەنجەی ملی منداڵدان.',
    value: PAP_SMEAR_EVERY_FIVE_YEARS,
    type: CERVICAL
  },
  {name: 'پشکنینی پیسایی (FIT) هەموو ساڵ جارێک بۆ شێرپەنجەی کۆلۆن.', value: FIT, type: COLON},
  {
    name: 'پشکنینی پیسایی (FIT) هەموو ساڵ جارێک یان ئەنجامدانی نازوری کۆڵۆن هەموو ١٠ ساڵ جارێک بۆ شێرپەنجەی کۆلۆن.',
    value: COLONOSCOPY_EVRY_TEN_YEAR,
    type: COLON
  },
  {
    name: 'ئەنجامدانی نازوری کۆڵۆن هەموو ٥ ساڵ جارێک بۆ شێرپەنجەی کۆلۆن.',
    value: COLONOSCOPY_EVRY_FIVE_YEAR,
    type: COLON
  },
  {name: 'لە ئێستادا پێویستت بە هیچ پشکنینێکی پێشوەختە نییە بۆ شێرپەنجە باوەکان.', value: NO_TESTS_REQUIRED, type: ALL},
];
const hospitals = [
  {name: 'نەخۆشخانەی ڕزگاری فێرکاری لە هەولێر.', value: RIZGARY},
  {name: 'سەنتەری مەمک لە هەولێر (بنکەی تەندروستی نازدار بامەڕنی).', value: BREAST_CENTER},
  {name: 'نەخۆشخانەی ئافرەتان و لەدایک بوونی فێرکاری لە هەولێر.', value: MATERNITY_HOSPITAL},
  {name: 'نەخۆشخانەی شار لە سلێمانی.', value: SHAR},
  {name: 'نەخۆشخانەی ئازادی لە دهۆک.', value: AZADI},
];


// Fields
const city = ref(HAWLER);
const age = ref();
const gender = ref(MALE);
const maritalStatus = ref(SINGLE);
const smoking = ref(NON_SMOKER);
const fmhx = ref([]);
const hx = ref([]);
const suggestedInvestigations = ref([]);
const suggestedHospitals = ref([]);


watch(gender, async (newGender, oldGender) => {
  hx.value = []
})


function checkCancerByGender(cancer) {
  switch (cancer) {
    case BREAST:
    case CERVICAL:
    case OVARIAN:
      return gender.value === FEMALE;
    case PROSTATE:
      return gender.value === MALE;
    default:
      return true
  }
}


function analyse(e) {
  if (typeof age.value == 'undefined') {
    alert('تکایە تەمەن بنووسە!');
    return false;
  }

  suggestedInvestigations.value = [];
  suggestedHospitals.value = [];

  if (gender.value === FEMALE) {
    checkBreastCancer();
    checkCervicalCancer();
  }

  if (gender.value === MALE) {
    checkProstateCancer();
  }

  checkLungCancer();
  checkColonCancer();

  if (suggestedInvestigations.value.length > 0) {
    setSuggestedHospitals();
  }

  if (suggestedInvestigations.value.length === 0) {
    suggestedInvestigations.value = [NO_TESTS_REQUIRED];
  }

}

function checkBreastCancer() {
  const oldInvestigations = suggestedInvestigations.value;
  if (age.value >= 20 && age.value < 45) {
    suggestedInvestigations.value = [...oldInvestigations, SELF_EXAMINATION];
  }

  if (age.value >= 45 && age.value <= 69) {
    suggestedInvestigations.value = [...oldInvestigations, MAMMOGRAPHY_EVERY_TOW_YEAR];
  }

  // high risk
  const hasFamilyHxOfCancers = fmhx.value
      .filter((item, index) => [BREAST, CERVICAL, OVARIAN].includes(item))
      .length > 0;

  if ((age.value >= 30 && age.value <= 40) && (hx.value.length > 0 || hasFamilyHxOfCancers)) {
    suggestedInvestigations.value = [...oldInvestigations, CLINICAL_EXAMINATION];
  }

  if ((age.value >= 40 && age.value <= 69) && (hx.value.length > 0 || hasFamilyHxOfCancers)) {
    suggestedInvestigations.value = [...oldInvestigations, MAMMOGRAPHY_ANNUALLY];
  }
}

function checkCervicalCancer() {
  const oldInvestigations = suggestedInvestigations.value;
  if (age.value >= 25 && age.value <= 49 && maritalStatus.value === MARRIED) {
    suggestedInvestigations.value = [...oldInvestigations, PAP_SMEAR_EVERY_THREE_YEARS];
  }
  if (age.value >= 50 && age.value <= 69 && maritalStatus.value === MARRIED) {
    suggestedInvestigations.value = [...oldInvestigations, PAP_SMEAR_EVERY_FIVE_YEARS];
  }
}

function checkProstateCancer() {
  const oldInvestigations = suggestedInvestigations.value;
  if ((age.value >= 55 && age.value <= 72)) {
    suggestedInvestigations.value = [...oldInvestigations, PSA_OR_DRE];
  }

  if ((age.value >= 45 && age.value <= 72) && (hx.value.length > 0 || fmhx.value.length > 0)) {
    suggestedInvestigations.value = [...oldInvestigations, PSA_OR_DRE];
  }

}

function checkLungCancer() {
  const oldInvestigations = suggestedInvestigations.value;
  const hasFamilyHxOfCancers = fmhx.value
      .filter((item, index) => [LUNG].includes(item))
      .length > 0;
  if ((age.value >= 55 && age.value <= 70) && (smoking.value === SMOKER || (hx.value.length > 0 || hasFamilyHxOfCancers))) {
    suggestedInvestigations.value = [...oldInvestigations, CHEST_XRAY];
  }
}

function checkColonCancer() {
  const oldInvestigations = suggestedInvestigations.value;
  if ((age.value >= 45 && age.value <= 75)) {
    suggestedInvestigations.value = [...oldInvestigations, COLONOSCOPY_EVRY_TEN_YEAR];
  }

  const hasFamilyHxOfCancers = fmhx.value
      .filter((item, index) => [COLON, PROSTATE, PANCREATIC].includes(item))
      .length > 0;

  if ((age.value >= 45 && age.value <= 75) && (hx.value.length > 0 || hasFamilyHxOfCancers)) {
    suggestedInvestigations.value = [...oldInvestigations, COLONOSCOPY_EVRY_FIVE_YEAR];
  }
}


function setSuggestedHospitals() {
  switch (city.value) {
    case HAWLER:
      setHawlerHospitals()
      break;
    case SULAYMANI:
      suggestedHospitals.value = [SHAR];
      break;
    case DUHOK:
      suggestedHospitals.value = [AZADI];
      break;
  }
}

function setHawlerHospitals() {
  suggestedHospitals.value = [RIZGARY];

  const hasBreastTests = suggestedInvestigations.value
      .filter((item) => [
            SELF_EXAMINATION,
            CLINICAL_EXAMINATION,
            MAMMOGRAPHY_EVERY_TOW_YEAR,
            MAMMOGRAPHY_ANNUALLY
          ].includes(item)
      ).length > 0;

  if (hasBreastTests) {
    suggestedHospitals.value = [...suggestedHospitals.value, BREAST_CENTER];
  }

  const hasCervicalTests = suggestedInvestigations.value
      .filter((item) => [
            PAP_SMEAR_EVERY_THREE_YEARS,
            PAP_SMEAR_EVERY_FIVE_YEARS,
          ].includes(item)
      ).length > 0;

  if (hasCervicalTests) {
    suggestedHospitals.value = [...suggestedHospitals.value, MATERNITY_HOSPITAL]
  }
}
</script>