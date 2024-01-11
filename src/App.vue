<script  setup>
import HelloWorld from './components/HelloWorld.vue'
import TheWelcome from './components/TheWelcome.vue'
import CountryCard from './components/CountryCard.vue'
import { FwbCard } from 'flowbite-vue'
import { FwbPagination } from 'flowbite-vue'
import {
  FwbA,
  FwbTable,
  FwbTableBody,
  FwbTableCell,
  FwbTableHead,
  FwbTableHeadCell,
  FwbTableRow,
} from 'flowbite-vue'
import { ref, onMounted } from 'vue';
import { FwbButton, FwbInput } from 'flowbite-vue'

const query = ref('')
const currentPage = ref(1)


const countries = ref([]);
// const counties = ref([])

onMounted(async () => {
  try {
    const response = await fetch(' https://restcountries.com/v3.1/all');
    const result = await response.json();
    countries.value = result;
    // console.log("asdf" + data.value);
  } catch (error) {
    console.error('Error fetching data:', error);
  }
});
</script>

<template>
  <!-- <header>
    <img alt="Vue logo" class="logo" src="./assets/logo.svg" width="125" height="125" />

    <div class="wrapper">
      <HelloWorld msg="You did it!" />
    </div>
  </header>

  <main>
    <TheWelcome />
  </main> -->

  <div class="">
    <h1 class="text-center text-[2rem] font-bold tracking-wider pt-10">Country all round the world!</h1>
    <div class="w-full flex justify-center">
      <div class="w-[90%] flex justify-end">
        <div class="w-2/5">
          <div class="py-10">
            <fwb-input v-model="query" label="" placeholder="Search your country" size="lg">
              <template #prefix>
                <svg aria-hidden="true" class="w-5 h-5 text-gray-500 dark:text-gray-400" fill="none" stroke="currentColor"
                  viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg">
                  <path d="M21 21l-6-6m2-5a7 7 0 11-14 0 7 7 0 0114 0z" stroke-linecap="round" stroke-linejoin="round"
                    stroke-width="2" />
                </svg>
              </template>
              <template #suffix>
                <fwb-button>Search</fwb-button>
              </template>
            </fwb-input>
          </div>
        </div>

      </div>
    </div>

    <div class="flex justify-center tracking-wide">
      <fwb-table class="w-[90%]">
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

        <fwb-table-body class="text-[1rem]">
          <fwb-table-row v-for="country, index in countries" :key="index">
            <fwb-table-cell>{{ index + 1 }}</fwb-table-cell>
            <fwb-table-cell class="w-[15%]">
              <img :src="country.flags.png" alt="" class="border rounded-lg">
            </fwb-table-cell>
            <fwb-table-cell>{{ country.name.official }}</fwb-table-cell>
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


    <center>
      <fwb-pagination v-model="currentPage" :total-items="100" class="py-10"></fwb-pagination>
    </center>

  </div>
</template>

<style></style>
