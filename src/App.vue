<script setup>
import {
  FwbButton, FwbInput, FwbModal, FwbPagination, FwbTable,
  FwbTableBody,
  FwbTableCell,
  FwbTableHead,
  FwbTableHeadCell,
  FwbTableRow,
} from 'flowbite-vue'
import { onMounted, ref, watch } from 'vue'
import Popup from './components/Popup.vue';
import SortSelect from './components/SortSelect.vue';
import Search from './components/Search.vue';

const currentPage = ref(1);
const itemsPerPage = 25;
const countries = ref([]);
const splitCountries = ref([]);
const availableItem = ref(0)
const country = ref('')
const totalShow = ref(0)
const isLoading = ref(true)

watch(currentPage, (newPage, oldPage) => {
  if (newPage > oldPage) {
    totalShow.value = (itemsPerPage * (newPage - 1)) + splitCountries.value[newPage - 1].length
  } else {
    totalShow.value = newPage * itemsPerPage
  }
});

// sorting
const sortOption = ref('def');
const dropdownOptions = ref([
  { value: "def", label: "Default" },
  { value: "acs", label: "ACS" },
  { value: "desc", label: "DESC" },
])

// for showing popup
const isShowPopup = ref(false)

function closePopup() {
  isShowPopup.value = false
}

function showPopup(inputCountry) {
  country.value = inputCountry
  isShowPopup.value = true
}

onMounted(async () => {
  try {
    const response = await fetch('https://restcountries.com/v3.1/all');
    const result = await response.json();
    countries.value = result;

    splitCountries.value = splitCountriesIntoSubarrays(countries.value);
    availableItem.value = countries.value.length
    totalShow.value = splitCountries.value[currentPage.value - 1].length * (currentPage.value)
    isLoading.value = false
  } catch (error) {
    console.error('Error fetching data:', error);
  }
});

const splitCountriesIntoSubarrays = (data) => {
  const tmpArray = [...data];
  const tmp = [];

  while (tmpArray.length > 0) {
    tmp.push(tmpArray.splice(0, itemsPerPage));
  }
  return tmp
};


// for searching
const searchTerm = ref('');

const performSearch = (input) => {
  console.log(input);
  const tmp = countries.value.filter(item => {
    if (item.name.official.toLowerCase().includes(input.toLowerCase())) {
      return item
    }
  })
  availableItem.value = tmp.length
  currentPage.value = 1
  splitCountries.value = splitCountriesIntoSubarrays(tmp)
  if (tmp.length == 0) {
    availableItem.value = 0
    totalShow.value = 0
  } else {
    totalShow.value = splitCountries.value[currentPage.value - 1].length * (currentPage.value)
  }

};


// sort function
const sortCountries = (option) => {
  const tmp = [...countries.value];
  if (option == 'desc') {
    tmp.sort((a, b) => (a.name.official > b.name.official ? -1 : 1));
  } else if (option == "acs") {
    tmp.sort((a, b) => (a.name.official > b.name.official ? 1 : -1));
  }

  // After sorting, update the splitCountries
  availableItem.value = tmp.length
  currentPage.value = 1
  splitCountries.value = splitCountriesIntoSubarrays(tmp)
  totalShow.value = splitCountries.value[currentPage.value - 1].length * (currentPage.value)
};

</script>

<template>
  <div class="">
    <h1 class="text-center text-[2rem] text-gary-800 font-bold tracking-wider pt-10">Let's Go to See Countries</h1>
    <div class="w-full flex justify-center pt-10">
      <div class="w-[90%] flex justify-end gap-4">

        <!-- sort -->
        <SortSelect :option_selected="sortOption" :options="dropdownOptions" @change_option="sortCountries">
        </SortSelect>
        <!-- end sort -->

        <!-- search -->
        <div class="grow sm:grow-0 sm:w-[200px] xl:w-1/5">
          <Search :search-term="searchTerm" @searching="performSearch"></Search>
        </div>
        <!-- end search -->

      </div>
    </div>

    <!-- count -->
    <div class="flex justify-center w-full py-6">
      <div class="w-[90%] flex justify-end text-gray-700">
        <p class="text-[1.2rem] tracking-wide font-medium">{{ totalShow }} / {{ availableItem }} Countries</p>
      </div>
    </div>
    <!-- end count -->

    <!-- loading -->
    <div v-if="isLoading" class="flex justify-center items-center pt-[100px]">
      <div>
        <span class="loader"></span>
      </div>
    </div>
    <!-- end loading -->

    <!-- table -->
    <div v-else class="flex justify-center tracking-wide overflow h-[60vh]">
      <fwb-table class="w-[90%]" striped>
        <fwb-table-head class="text-[1.2rem]">
          <fwb-table-head-cell class="">No</fwb-table-head-cell>
          <fwb-table-head-cell class="">Flag</fwb-table-head-cell>
          <fwb-table-head-cell class="">Offical Name</fwb-table-head-cell>
          <fwb-table-head-cell class="">cca2</fwb-table-head-cell>
          <fwb-table-head-cell class="">cca3</fwb-table-head-cell>
          <fwb-table-head-cell class="">Native Name</fwb-table-head-cell>
          <fwb-table-head-cell class="">Alternative Country Name</fwb-table-head-cell>
          <fwb-table-head-cell class="">Code</fwb-table-head-cell>
        </fwb-table-head>

        <fwb-table-body class="text-[1rem] max-h-[60vh]">
          <fwb-table-row v-for="country, index in splitCountries[currentPage - 1]" :key="index">
            <fwb-table-cell class="">{{ (itemsPerPage * (currentPage - 1)) + index + 1 }}</fwb-table-cell>
            <fwb-table-cell class="min-w-[230px] max-w-[230px] w-[230px]">
              <img :src="country.flags.png" alt="" class="border w-full rounded-lg">
            </fwb-table-cell>
            <fwb-table-cell>
              <button @click="showPopup(country)" class="font-bold text-left">
                {{ country.name.official }}
              </button>
            </fwb-table-cell>
            <fwb-table-cell>{{ country.cca2 }}</fwb-table-cell>
            <fwb-table-cell>{{ country.cca3 }}</fwb-table-cell>
            <fwb-table-cell>
              <div class="text-start overflow-auto h-[100px] flex items-center">
                <div>
                  <p v-for="(name, langCode) in country.name.nativeName" :key="langCode">
                    <span class="font-medium">{{ langCode.toUpperCase() }}:</span> {{ name.official }}
                  </p>
                </div>
              </div>
            </fwb-table-cell>
            <fwb-table-cell>
              <div class="text-start overflow-auto h-[110px] flex items-center">
                <div>
                  <p v-for="altName in country.altSpellings" :key="altName">
                    . {{ altName }}
                  </p>
                </div>

              </div>
            </fwb-table-cell>

            <fwb-table-cell class="w-[10%]">
              <div class="text-start overflow-auto h-[100px] flex items-center">
                <div>
                  <p v-for="(suf, index) in country.idd.suffixes" :key="index">
                    {{ country.idd.root }}{{ suf }}
                  </p>
                </div>
              </div>
            </fwb-table-cell>
          </fwb-table-row>
        </fwb-table-body>
      </fwb-table>
    </div>
    <!-- end table -->

    <!-- pagination -->
    <center>
      <fwb-pagination v-model="currentPage" :total-items="availableItem" :perPage="itemsPerPage"
        class="py-10"></fwb-pagination>
    </center>
    
    <!-- end pagination -->

  </div>

  <!-- modal 2 -->
  <Popup :country="country" :isShow="isShowPopup" @close="closePopup"></Popup>
  <!-- end modal 2 -->
</template>

<style>
.loader {
  width: 48px;
  height: 48px;
  border-radius: 50%;
  display: inline-block;
  position: relative;
  border: 10px solid;
  border-color: rgba(0, 0, 0, 0.15) rgba(0, 0, 0, 0.25) rgba(0, 0, 0, 0.35) rgba(0, 0, 0, 0.5);
  box-sizing: border-box;
  animation: rotation 1s linear infinite;
}

@keyframes rotation {
  0% {
    transform: rotate(0deg);
  }

  100% {
    transform: rotate(360deg);
  }
}
</style>



