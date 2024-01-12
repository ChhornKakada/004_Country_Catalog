<script  setup>
import {
  FwbButton, FwbInput, FwbModal, FwbPagination, FwbTable,
  FwbTableBody,
  FwbTableCell,
  FwbTableHead,
  FwbTableHeadCell,
  FwbTableRow,

} from 'flowbite-vue'
import { onMounted, ref } from 'vue'
import Popup from './components/Popup.vue';



const currentPage = ref(1);
const itemsPerPage = 25;
const countries = ref([]);
const splitCountries = ref([]);
const availableItem = ref(0)
const country = ref('')

const sortOption = ref('def');

const isShowPopup = ref(false)

function closePopup() {
  isShowPopup.value = false
}

function showPopup(countryName) {
  for (var item of countries.value) {
    if (item.name.official == countryName) {
      country.value = item
      break
    }
  }
  isShowPopup.value = true
}

onMounted(async () => {
  try {
    const response = await fetch('https://restcountries.com/v3.1/all');
    const result = await response.json();
    countries.value = result;

    splitCountries.value = splitCountriesIntoSubarrays(countries.value);
    availableItem.value = countries.value.length
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

const searchTerm = ref('');

const performSearch = () => {
  const tmp = countries.value.filter(item => {
    if (item.name.official.toLowerCase().includes(searchTerm.value.toLowerCase())) {
      return item
    }
  })
  availableItem.value = tmp.length
  currentPage.value = 1
  splitCountries.value = splitCountriesIntoSubarrays(tmp)
};

const sortCountries = () => {
  const tmp = [...countries.value];
  if (sortOption.value == 'desc') {
    tmp.sort((a, b) => (a.name.official > b.name.official ? -1 : 1));
  } else if (sortOption.value == "acs") {
    tmp.sort((a, b) => (a.name.official > b.name.official ? 1 : -1));
  }

  // After sorting, update the splitCountries
  availableItem.value = tmp.length
  currentPage.value = 1
  splitCountries.value = splitCountriesIntoSubarrays(tmp)
};

</script>

<template>
  <div class="">
    <h1 class="text-center text-[2rem] font-bold tracking-wider pt-10">Country all round the world!</h1>
    <!-- <p>{{ isShowModal }}</p> -->

    <div class="w-full flex justify-center pt-10">
      <div class="w-[90%] flex justify-end gap-4">

        <!-- sort -->
        <div class="flex gap-2 items-center text-xl">
          <div class="flex">
            <button id="states-button" data-dropdown-toggle="dropdown-states"
              class="flex-shrink-0 z-10 inline-flex items-center py-2.5 px-4 text-sm font-medium text-center text-gray-500 bg-gray-100 border border-gray-300 rounded-s-lg hover:bg-gray-200 focus:ring-4 focus:outline-none focus:ring-gray-100 dark:bg-gray-700 dark:hover:bg-gray-600 dark:focus:ring-gray-700 dark:text-white dark:border-gray-600"
              type="button">

              <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5"
                stroke="currentColor" class="w-6 h-6">
                <path stroke-linecap="round" stroke-linejoin="round" d="M3.75 6.75h16.5M3.75 12H12m-8.25 5.25h16.5" />
              </svg>
              <span class="pl-2">Sort by</span>
            </button>

            <select id="states" v-model="sortOption" @change="sortCountries"
              class="bg-gray-50 border border-gray-300 text-gray-900 text-sm rounded-e-lg border-s-gray-100 dark:border-s-gray-700 border-s-2 focus:ring-blue-500 focus:border-blue-500 block w-full p-2.5 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500">
              <option selected value="def">Default</option>
              <option value="desc">DESC</option>
              <option value="acs">ASC</option>
            </select>
          </div>
        </div>
        <!-- end sort -->


        <!-- search -->
        <div class="w-1/5">
          <div class="flex items-center">
            <label for="simple-search" class="sr-only">Search</label>
            <div class="relative w-full">
              <div class="absolute inset-y-0 start-0 flex items-center ps-3 pointer-events-none">
                <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="currentColor" class="w-6 h-6">
                  <path fill-rule="evenodd"
                    d="M10.5 3.75a6.75 6.75 0 1 0 0 13.5 6.75 6.75 0 0 0 0-13.5ZM2.25 10.5a8.25 8.25 0 1 1 14.59 5.28l4.69 4.69a.75.75 0 1 1-1.06 1.06l-4.69-4.69A8.25 8.25 0 0 1 2.25 10.5Z"
                    clip-rule="evenodd" />
                </svg>
              </div>
              <input type="text" id="simple-search" v-model="searchTerm" @input="performSearch" label=""
                class=" bg-gray-50 border border-gray-300 text-gray-900 text-sm rounded-lg focus:ring-blue-500 focus:border-blue-500 block w-full ps-10 p-2.5 py-[12px]  dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500"
                placeholder="Search your country..." required>
            </div>
          </div>
        </div>
        <!-- end search -->

      </div>
    </div>

    <!-- count -->
    <div class="flex justify-center w-full py-6">
      <div class="w-[90%] flex justify-end">
        <p class="text-[1.2rem] tracking-wide font-medium"></p>
      </div>
    </div>
    <!-- end count -->

    <!-- table -->
    <div class="flex justify-center tracking-wide overflow-auto h-[60vh]">
      <fwb-table class="w-[90%]" striped>
        <fwb-table-head class="text-[1.2rem] w-full">
          <fwb-table-head-cell class="">No</fwb-table-head-cell>
          <fwb-table-head-cell class="text-center ">Flag</fwb-table-head-cell>
          <fwb-table-head-cell class="">official Name</fwb-table-head-cell>
          <fwb-table-head-cell class="">cca2</fwb-table-head-cell>
          <fwb-table-head-cell class="">cca3</fwb-table-head-cell>
          <fwb-table-head-cell class="">Native Name</fwb-table-head-cell>
          <fwb-table-head-cell class="">Alternative Country Name</fwb-table-head-cell>
          <fwb-table-head-cell class="">Code</fwb-table-head-cell>
        </fwb-table-head>

        <fwb-table-body class="text-[1rem] max-h-[60vh] overflow-auto">
          <fwb-table-row v-for="country, index in splitCountries[currentPage - 1]" :key="index">
            <fwb-table-cell class="">{{ (itemsPerPage * (currentPage - 1)) + index + 1 }}</fwb-table-cell>
            <fwb-table-cell class="min-w-[220px]">
              <img :src="country.flags.png" alt="" class="border w-full rounded-lg">
            </fwb-table-cell>
            <fwb-table-cell>
              <button @click="showPopup(country.name.official)" class="font-bold text-left">
                {{ country.name.official }}
              </button>
            </fwb-table-cell>
            <fwb-table-cell>{{ country.cca2 }}</fwb-table-cell>
            <fwb-table-cell>{{ country.ccn3 }}</fwb-table-cell>
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
              <p v-for="altName in country.altSpellings" :key="altName">
                {{ altName }}
              </p>
            </fwb-table-cell>

            <fwb-table-cell class="w-[10%]">
              <div class="text-start overflow-auto h-[100px] flex items-center">
                <div>
                  <p v-for="(name, langCode, index) in country.idd" :key="index">
                    {{ langCode.toUpperCase() }}:
                    <span v-if="index == 1">
                      <span v-for="v, index in name" :key="index">
                        <span v-if="index > 0">, {{ v }}</span>
                        <span v-else>{{ v }} </span>
                      </span>
                    </span>
                    <span v-else>{{ name }}</span>
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
    <!-- <iframe src="https://www.google.com/maps/embed?pb=nztQtFSrUXZymJaW8" width="600" height="450" style="border:0;" allowfullscreen="" loading="lazy" referrerpolicy="no-referrer-when-downgrade"></iframe> -->
    <!-- end pagination -->

  </div>

  <!-- modal 2 -->
  <Popup :country="country" :isShow="isShowPopup" @close="closePopup"></Popup>
  <!-- end modal 2 -->
</template>

<style></style>
