<script  setup>
import {
  FwbButton, FwbInput, FwbModal, FwbPagination, FwbTable,
  FwbTableBody,
  FwbTableCell,
  FwbTableHead,
  FwbTableHeadCell,
  FwbTableRow
} from 'flowbite-vue'
import { onMounted, ref } from 'vue'


const currentPage = ref(1);
const itemsPerPage = 25;
const countries = ref([]);
const splitCountries = ref([]);
const availableItem = ref(0)

const sortOption = ref('def');


const isShowModal = ref(false)

function closeModal() {
  isShowModal.value = false
}
function showModal() {
  isShowModal.value = true
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
const searchResults = ref([]);

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
    <div class="w-full flex justify-center pt-10">
      <div class="w-[90%] flex justify-end gap-4">
        <div class="flex gap-2 items-center text-xl">
          <label for="countries" class="block font-medium text-gray-900 dark:text-white w-[130px]">Sort by</label>
          <select id="countries" v-model="sortOption" @change="sortCountries"
            class="bg-gray-50 border border-gray-300 text-gray-900 text-lg rounded-lg focus:ring-blue-500 focus:border-blue-500 block w-full p-[11px] dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500">
            <option selected value="def">Default</option>
            <option value="desc">DESC</option>
            <option value="acs">ASC</option>
          </select>

        </div>
        <div class="w-2/5">
          <div class="">
            <fwb-input v-model="searchTerm" @input="performSearch" label="" class="text-[1.2rem]"
              placeholder="Search your country" size="lg">
              <template #prefix>
                <svg aria-hidden="true" class="w-5 h-5 text-gray-500 dark:text-gray-400" fill="none" stroke="currentColor"
                  viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg">
                  <path d="M21 21l-6-6m2-5a7 7 0 11-14 0 7 7 0 0114 0z" stroke-linecap="round" stroke-linejoin="round"
                    stroke-width="2" />
                </svg>
              </template>
            </fwb-input>
          </div>
        </div>
      </div>
    </div>

    <div class="flex justify-center w-full py-6">
      <div class="w-[90%] flex justify-end">
        <!-- <p class="text-[1.2rem] tracking-wide font-medium">{{ splitCountries[currentPage].length * currentPage }} / {{ availableItem.length }} Countries</p> -->
      </div>
    </div>


    <div class="flex justify-center tracking-wide">
      <fwb-table class="w-[90%]" striped>
        <fwb-table-head class="text-[1.2rem]">
          <fwb-table-head-cell class="">No</fwb-table-head-cell>
          <fwb-table-head-cell class="text-center">Flag</fwb-table-head-cell>
          <fwb-table-head-cell>official Name</fwb-table-head-cell>
          <fwb-table-head-cell>cca2</fwb-table-head-cell>
          <fwb-table-head-cell>cca3</fwb-table-head-cell>
          <fwb-table-head-cell>Native Name</fwb-table-head-cell>
          <fwb-table-head-cell>Alternative Country Name</fwb-table-head-cell>
          <fwb-table-head-cell>Code</fwb-table-head-cell>
        </fwb-table-head>

        <fwb-table-body class="text-[1rem] max-h-[60vh] overflow-auto">
          <fwb-table-row v-for="country, index in splitCountries[currentPage - 1]" :key="index">
            <fwb-table-cell class="">{{ index + 1 }}</fwb-table-cell>
            <fwb-table-cell class="w-[15%]">
              <img :src="country.flags.png" alt="" class="border rounded-lg">
            </fwb-table-cell>
            <fwb-table-cell><button @click="showModal">{{ country.name.official }}</button></fwb-table-cell>
            <!-- <fwb-table-cell>{{ country.cca2 }}</fwb-table-cell> -->
            <fwb-table-cell>
              <button data-modal-target="static-modal" data-modal-toggle="static-modal" type="button">
                {{ country.cca2 }}
              </button>
            </fwb-table-cell>
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

    <center>
      <fwb-pagination v-model="currentPage" :total-items="availableItem" :perPage="itemsPerPage"
        class="py-10"></fwb-pagination>
    </center>

  </div>

  <!-- modal -->
  <fwb-modal v-if="isShowModal" @close="closeModal" class="border-2 border-red-500">
    <template #header>
      <div class="flex items-center text-lg border-2">
        Terms of Service
      </div>
    </template>
    <template #body>
      <p class="text-base leading-relaxed text-gray-500 dark:text-gray-400">
        With less than a month to go before the European Union enacts new consumer privacy laws for its citizens,
        companies around the world are updating their terms of service agreements to comply.
      </p>
      <p class="text-base leading-relaxed text-gray-500 dark:text-gray-400">
        The European Union’s General Data Protection Regulation (G.D.P.R.) goes into effect on May 25 and is meant to
        ensure a common set of data rights in the European Union. It requires organizations to notify users as soon as
        possible of high-risk data breaches that could personally affect them.
      </p>
    </template>
    <template #footer>
      <div class="flex justify-between">
        <fwb-button @click="closeModal" color="alternative">
          Decline
        </fwb-button>
        <fwb-button @click="closeModal" color="green">
          I accept
        </fwb-button>
      </div>
    </template>
  </fwb-modal>
  <!-- end modal -->


  <!-- modal 2 -->
  <div id="static-modal" data-modal-backdrop="static" tabindex="-1" aria-hidden="true"
    class="hidden overflow-y-auto overflow-x-hidden fixed top-0 right-0 left-0 z-50 justify-center items-center w-full md:inset-0 h-[calc(100%-1rem)] max-h-full">
    <div class="relative p-4 w-full max-w-2xl max-h-full">
      <!-- Modal content -->
      <div class="relative bg-white rounded-lg shadow dark:bg-gray-700">
        <!-- Modal header -->
        <div class="flex items-center justify-between p-4 md:p-5 border-b rounded-t dark:border-gray-600">
          <h3 class="text-xl font-semibold text-gray-900 dark:text-white">
            Static modal
          </h3>
          <button type="button"
            class="text-gray-400 bg-transparent hover:bg-gray-200 hover:text-gray-900 rounded-lg text-sm w-8 h-8 ms-auto inline-flex justify-center items-center dark:hover:bg-gray-600 dark:hover:text-white"
            data-modal-hide="static-modal">
            <svg class="w-3 h-3" aria-hidden="true" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 14 14">
              <path stroke="currentColor" stroke-linecap="round" stroke-linejoin="round" stroke-width="2"
                d="m1 1 6 6m0 0 6 6M7 7l6-6M7 7l-6 6" />
            </svg>
            <span class="sr-only">Close modal</span>
          </button>
        </div>
        <!-- Modal body -->
        <div class="p-4 md:p-5 space-y-4">
          <p class="text-base leading-relaxed text-gray-500 dark:text-gray-400">
            With less than a month to go before the European Union enacts new consumer privacy laws for its citizens,
            companies around the world are updating their terms of service agreements to comply.
          </p>
          <p class="text-base leading-relaxed text-gray-500 dark:text-gray-400">
            The European Union’s General Data Protection Regulation (G.D.P.R.) goes into effect on May 25 and is meant to
            ensure a common set of data rights in the European Union. It requires organizations to notify users as soon as
            possible of high-risk data breaches that could personally affect them.
          </p>
        </div>
        <!-- Modal footer -->
        <div class="flex items-center p-4 md:p-5 border-t border-gray-200 rounded-b dark:border-gray-600">
          <button data-modal-hide="static-modal" type="button"
            class="text-white bg-blue-700 hover:bg-blue-800 focus:ring-4 focus:outline-none focus:ring-blue-300 font-medium rounded-lg text-sm px-5 py-2.5 text-center dark:bg-blue-600 dark:hover:bg-blue-700 dark:focus:ring-blue-800">I
            accept</button>
          <button data-modal-hide="static-modal" type="button"
            class="ms-3 text-gray-500 bg-white hover:bg-gray-100 focus:ring-4 focus:outline-none focus:ring-blue-300 rounded-lg border border-gray-200 text-sm font-medium px-5 py-2.5 hover:text-gray-900 focus:z-10 dark:bg-gray-700 dark:text-gray-300 dark:border-gray-500 dark:hover:text-white dark:hover:bg-gray-600 dark:focus:ring-gray-600">Decline</button>
        </div>
      </div>
    </div>
  </div>
  <!-- end modal 2 -->
</template>

<style></style>
