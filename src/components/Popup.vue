<script>
import IconBuildingLibrary from './icons/IconBuildingLibrary.vue';
import IconIdentification from './icons/IconIdentification.vue';
import IconLanguage from './icons/IconLanguage.vue';
import IconMapPin from './icons/IconMapPin.vue';
import IconRectangleStack from './icons/IconRectangleStack.vue';
import IconUsers from './icons/IconUsers.vue';
import IconFlag from './icons/IconFlag.vue';
import IconEarth from './icons/IconEarth.vue';
import IconClock from './icons/IconClock.vue';
import IconPhone from './icons/IconPhone.vue';
import IconDollar from './icons/IconDollar.vue';


export default {
  name: "Popup",
  props: {
    country: Object || String,
    isShow: Boolean
  },
  methods: {
    close() {
      this.$emit('close');
    }
  },
  components: {
    IconBuildingLibrary,
    IconIdentification,
    IconLanguage,
    IconUsers,
    IconRectangleStack,
    IconMapPin,
    IconEarth,
    IconFlag,
    IconClock,
    IconPhone,
    IconDollar
  }
};
</script>

<!-- html -->
<template>
  <div v-if="country != ''">
    <div v-if="isShow" class="fixed top-0 right-0 left-0 bottom-0 bg-black opacity-80 z-40"></div>
    <div id="static-modal" data-modal-backdrop="static" tabindex="-1" aria-hidden="true"
      :class="{ 'flex': isShow, 'hidden': !isShow }"
      class=" overflow-y-auto overflow-x-hidden fixed top-0 right-0 left-0 z-50 justify-center items-center w-full md:inset-0 h-[calc(100%-1rem)] max-h-full">
      <div
        class="relative p-4 flex justify-center items-center w-full max-w-[90%] md:max-w-[80%] xl:max-w-[75%] 2xl:max-w-[1200px] max-h-full h-[100vh]">
        <!-- Modal content -->
        <div class="px-8 py-4 relative h-[90vh] overflow-auto w-full bg-white rounded-lg shadow dark:bg-gray-700">
          <!-- Modal header -->
          <div class="flex items-center justify-between p-4 md:p-5 border-b rounded-t dark:border-gray-600">
            <div class="flex gap-3 justify-center items-center font-semibold text-gray-900 dark:text-white text-[2rem]">
              <IconFlag></IconFlag>
              <h1 class="tracking-widest">{{ country.name.common }}</h1>
            </div>
            <button type="button" @click="close()"
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
          <div class="p-4 md:p-5  lg:flex items-start gap-8 ">

            <!-- flag -->
            <div class="lg:w-[40%] pb-10 lg:pb-0">
              <img :src="country.flags.png" class="w-full rounded-lg border border-black" alt="">
              <p class="text-gray-500 py-4 tracking-wide">{{ country.flags.alt }}</p>
              <div class="relative overflow-x-auto shadow-md sm:rounded-lg overflow-y-auto">
                <table class="w-full text-sm text-left rtl:text-right text-gray-500 dark:text-gray-400">
                  <tbody>
                    <!-- Offical name -->
                    <tr class="border-b border-gray-200 dark:border-gray-700">
                      <th scope="row"
                        class="px-6 py-4 font-medium text-gray-900 whitespace-nowrap bg-gray-50 dark:text-white dark:bg-gray-800">
                        <div class="flex items-center gap-2">
                          <p>CCA2</p>
                        </div>
                      </th>
                      <td class="px-6 py-4">
                        <p>
                          <span class="tracking-wide">{{ country.cca2 }}</span>
                        </p>
                      </td>
                      <th scope="row"
                        class="px-6 py-4 font-medium text-gray-900 whitespace-nowrap bg-gray-50 dark:text-white dark:bg-gray-800">
                        <div class="flex items-center gap-2">
                          <p>CCA3</p>
                        </div>
                      </th>
                      <td class="px-6 py-4">
                        <p>
                          <span class="tracking-wide">{{ country.cca3 }}</span>
                        </p>
                      </td>
                    </tr>
                    <!--  -->
                    <tr class="border-b border-gray-200 dark:border-gray-700">
                      <th scope="row"
                        class="px-6 py-4 font-medium text-gray-900 whitespace-nowrap bg-gray-50 dark:text-white dark:bg-gray-800">
                        <div class="flex items-center gap-2">
                          <p>CCN3</p>
                        </div>
                      </th>
                      <td class="px-6 py-4">
                        <p>
                          <span class="tracking-wide">{{ country.ccn3 }}</span>
                        </p>
                      </td>
                      <th scope="row"
                        class="px-6 py-4 font-medium text-gray-900 whitespace-nowrap bg-gray-50 dark:text-white dark:bg-gray-800">
                        <div class="flex items-center gap-2">
                          <p>CIOC</p>
                        </div>
                      </th>
                      <td class="px-6 py-4">
                        <p>
                          <span class="tracking-wide">{{ country.cioc }}</span>
                        </p>
                      </td>
                    </tr>
                  </tbody>
                </table>
              </div>
              <div class="pt-4">
                <p class="text-gray-500 font-medium dark:text-gray-400">Google map: </p>
                <a :href="country.maps.googleMaps"
                  class="font-medium text-blue-600 underline dark:text-blue-500 hover:no-underline  ">
                  <p class="overflow-hidden whitespace-nowrap overflow-ellipsis">
                    {{ country.maps.googleMaps }}
                  </p>
                </a>
              </div>
              <div class="pt-4 w-full">
                <p class="text-gray-500 font-medium dark:text-gray-400">Street map: </p>
                <a :href="country.maps.openStreetMaps"
                  class="font-medium text-blue-600 underline dark:text-blue-500 hover:no-underline  ">
                  <p class="overflow-hidden whitespace-nowrap overflow-ellipsis">

                    {{ country.maps.openStreetMaps }}
                  </p>
                </a>
              </div>
            </div>

            <!-- more info -->
            <div class="lg:w-[60%] ">
              <div class="relative overflow-x-auto shadow-md sm:rounded-lg overflow-y-auto">
                <table class="w-full text-sm text-left rtl:text-right text-gray-500 dark:text-gray-400">
                  <tbody>

                    <!-- Offical name -->
                    <tr class="border-b border-gray-200 dark:border-gray-700">
                      <th scope="row"
                        class="px-6 py-4 font-medium text-gray-900 whitespace-nowrap bg-gray-50 dark:text-white dark:bg-gray-800">
                        <div class="flex items-center gap-2">
                          <IconIdentification></IconIdentification>
                          <p>Offical name</p>
                        </div>
                      </th>
                      <td class="px-6 py-4">
                        <p>
                          <span class="tracking-wide">{{ country.name.official }}</span>
                        </p>
                      </td>
                    </tr>
                    <!-- end offical name -->

                    <!-- native name -->
                    <tr class="border-b border-gray-200 dark:border-gray-700">
                      <th scope="row"
                        class="px-6 py-4 font-medium text-gray-900 whitespace-nowrap bg-gray-50 dark:text-white dark:bg-gray-800">
                        <div class="flex items-center gap-2">
                          <IconRectangleStack></IconRectangleStack>
                          <p>Native name</p>
                        </div>
                      </th>
                      <td class="">
                        <div class="relative overflow-x-auto">
                          <table class="w-full text-sm text-left rtl:text-right text-gray-500 dark:text-gray-400">
                            <tbody>
                              <tr v-for="(name, langCode, index) in country.name.nativeName" :key="index"
                                :class="{ 'border-t': index != 0 }" class=" border-gray-200 dark:border-gray-700">
                                <th scope="row"
                                  class="px-6 py-2 font-medium text-gray-900 whitespace-nowrap bg-gray-50 dark:text-white dark:bg-gray-800">
                                  {{ langCode.toUpperCase() }}
                                </th>
                                <td class="px-6 py-2">
                                  {{ name.official }}
                                </td>
                              </tr>
                            </tbody>
                          </table>
                        </div>
                      </td>
                    </tr>
                    <!-- end native name -->

                    <!-- location -->
                    <tr class="border-b border-gray-200 dark:border-gray-700">
                      <th scope="row"
                        class="px-6 py-4 font-medium text-gray-900 whitespace-nowrap bg-gray-50 dark:text-white dark:bg-gray-800">
                        <div class="flex items-center gap-2">
                          <IconMapPin></IconMapPin>
                          <p>Location</p>
                        </div>
                      </th>
                      <td class="">
                        <div class="relative overflow-x-auto">
                          <table class="w-full text-sm text-left rtl:text-right text-gray-500 dark:text-gray-400">
                            <tbody>
                              <tr class=" border-gray-200 dark:border-gray-700">
                                <th scope="row"
                                  class="px-6 py-2 font-medium text-gray-900 whitespace-nowrap bg-gray-50 dark:text-white dark:bg-gray-800">
                                  Region
                                </th>
                                <td class="px-6 py-2">
                                  {{ country.region }}
                                </td>
                              </tr>
                              <tr class=" border-gray-200 dark:border-gray-700 border-t">
                                <th scope="row"
                                  class="px-6 py-2 font-medium text-gray-900 whitespace-nowrap bg-gray-50 dark:text-white dark:bg-gray-800">
                                  Subregion
                                </th>
                                <td class="px-6 py-2">
                                  {{ country.subregion }}
                                </td>
                              </tr>
                            </tbody>
                          </table>
                        </div>
                      </td>
                    </tr>
                    <!-- end location -->

                    <!-- code -->
                    <tr v-show="country.currencies" class="border-b border-gray-200 dark:border-gray-700">
                      <th scope="row"
                        class="px-6 py-4 font-medium text-gray-900 whitespace-nowrap bg-gray-50 dark:text-white dark:bg-gray-800">
                        <div class="flex items-center gap-2">
                          <IconDollar></IconDollar>
                          <p>Currency</p>
                        </div>
                      </th>
                      <td class="">
                        <div class="relative overflow-x-auto">
                          <table class="w-full text-sm text-left rtl:text-right text-gray-500 dark:text-gray-400">
                            <tbody>
                              <tr v-for="(data, key, index) in country.currencies" :key="index"
                                :class="{ 'border-t': index != 0 }" class=" border-gray-200 dark:border-gray-700">
                                <th scope="row"
                                  class="px-6 py-2 font-medium text-gray-900 whitespace-nowrap bg-gray-50 dark:text-white dark:bg-gray-800">
                                  {{ key.toUpperCase() }}
                                </th>
                                <td class="px-6 py-2">
                                  <div class="flex items-center">
                                    <div class="px-2 py-1 bg-gray-200 rounded-lg text-gray-900">{{ data.symbol }}</div>
                                    <div class="pl-4 whitespace-nowrap">{{ data.name }}</div>
                                  </div>
                                </td>
                              </tr>
                            </tbody>
                          </table>
                        </div>
                      </td>
                    </tr>
                    <!-- end code -->

                    <!-- capital -->
                    <tr class="border-b border-gray-200 dark:border-gray-700">
                      <th scope="row"
                        class="px-6 py-4 font-medium text-gray-900 whitespace-nowrap bg-gray-50 dark:text-white dark:bg-gray-800">
                        <div class="flex items-center gap-2">
                          <IconBuildingLibrary></IconBuildingLibrary>
                          <p>Capital</p>
                        </div>
                      </th>
                      <td class="px-6 py-4">
                        <span v-for="cap, index in country.capital" :key="index">
                          <span v-if="index > 0">, </span>
                          {{ cap }}
                        </span>
                      </td>
                    </tr>
                    <!--end capittal -->

                    <!-- population -->
                    <tr class="border-b border-gray-200 dark:border-gray-700">
                      <th scope="row"
                        class="px-6 py-4 font-medium text-gray-900 whitespace-nowrap bg-gray-50 dark:text-white dark:bg-gray-800">
                        <div class="flex items-center gap-2">
                          <IconUsers></IconUsers>
                          <p>Population</p>
                        </div>
                      </th>
                      <td class="px-6 py-4">
                        about. {{ country.population }}
                      </td>
                    </tr>
                    <!-- end population -->

                    <!-- languages -->
                    <tr class="border-b border-gray-200 dark:border-gray-700">
                      <th scope="row"
                        class="px-6 py-4 font-medium text-gray-900 whitespace-nowrap bg-gray-50 dark:text-white dark:bg-gray-800">
                        <div class="flex items-center gap-2">
                          <IconLanguage></IconLanguage>
                          <p>Languages</p>
                        </div>
                      </th>
                      <td class="px-6 py-4">
                        <span v-for="(name, langCode, index) in country.languages" :key="index">
                          <span v-if="index != 0">, </span> {{ name }}
                        </span>
                      </td>
                    </tr>
                    <!-- end languages -->

                    <!-- border -->
                    <tr class="border-b border-gray-200 dark:border-gray-700">
                      <th scope="row"
                        class="px-6 py-4 font-medium text-gray-900 whitespace-nowrap bg-gray-50 dark:text-white dark:bg-gray-800">
                        <div class="flex items-center gap-2">
                          <IconEarth></IconEarth>
                          <p>Border</p>
                        </div>
                      </th>
                      <td class="px-6 py-4">
                        <span v-for="border, index in country.borders" :key="index">
                          <span v-if="index != 0">, </span> {{ border }}
                        </span>
                      </td>
                    </tr>
                    <!-- end border -->

                    <!-- Timezone -->
                    <tr class="border-b border-gray-200 dark:border-gray-700">
                      <th scope="row"
                        class="px-6 py-4 font-medium text-gray-900 whitespace-nowrap bg-gray-50 dark:text-white dark:bg-gray-800">
                        <div class="flex items-center gap-2">
                          <IconClock></IconClock>
                          <p>Timezones</p>
                        </div>
                      </th>
                      <td class="px-6 py-4">
                        <span v-for="timezone, index in country.timezones" :key="index">
                          <span v-if="index != 0">, </span> {{ timezone }}
                        </span>
                      </td>
                    </tr>
                    <!-- end Timezone -->

                    <!-- code -->
                    <tr class="border-b border-gray-200 dark:border-gray-700">
                      <th scope="row"
                        class="px-6 py-4 font-medium text-gray-900 whitespace-nowrap bg-gray-50 dark:text-white dark:bg-gray-800">
                        <div class="flex items-center gap-2">
                          <IconPhone></IconPhone>
                          <p>Code</p>
                        </div>
                      </th>
                      <td class="px-6 py-4">
                        <span v-for="suf, index in country.idd.suffixes" :key="index">
                          <span v-if="index != 0">, </span> {{ country.idd.root }}{{ suf }}
                        </span>
                      </td>
                    </tr>
                    <!-- end code -->



                  </tbody>
                </table>
              </div>
            </div>
          </div>
          <!-- Modal footer -->
          <iframe src="https://goo.gl/maps/nztQtFSrUXZymJaW8" width="500" height="500" frameborder="0"></iframe>
          <iframe src="https://www.google.com/maps/embed?pb=!1m18!1m12!1m3!1d3988071.9708787063!2d103.93691332812504!3d12.542535381514385!2m3!1f0!2f0!3f0!3m2!1i1024!2i768!4f13.1!3m3!1m2!1s0x310787bfd4dc3743%3A0xe4b7bfe089f41253!2sCambodia!5e0!3m2!1sen!2skh!4v1705307019160!5m2!1sen!2skh" width="600" height="450" style="border:0;" allowfullscreen="" loading="lazy" referrerpolicy="no-referrer-when-downgrade"></iframe>
        </div>
      </div>
    </div>
  </div>
</template>

