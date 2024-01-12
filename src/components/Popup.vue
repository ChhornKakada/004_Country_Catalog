<script>

export default {
  name: "Popup",
  props: {
    country: Object || String,
    isShow: Boolean
  },
  methods: {
    close() {
      this.$emit('close')
    }
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
              <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="currentColor" class="w-8 h-8">
                <path fill-rule="evenodd"
                  d="M3 2.25a.75.75 0 0 1 .75.75v.54l1.838-.46a9.75 9.75 0 0 1 6.725.738l.108.054A8.25 8.25 0 0 0 18 4.524l3.11-.732a.75.75 0 0 1 .917.81 47.784 47.784 0 0 0 .005 10.337.75.75 0 0 1-.574.812l-3.114.733a9.75 9.75 0 0 1-6.594-.77l-.108-.054a8.25 8.25 0 0 0-5.69-.625l-2.202.55V21a.75.75 0 0 1-1.5 0V3A.75.75 0 0 1 3 2.25Z"
                  clip-rule="evenodd" />
              </svg>
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
              <div class="">
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
              <div class="relative overflow-x-auto shadow-md sm:rounded-lg">
                <table class="w-full text-sm text-left rtl:text-right text-gray-500 dark:text-gray-400">
                  <tbody>

                    <!-- Offical name -->
                    <tr class="border-b border-gray-200 dark:border-gray-700">
                      <th scope="row"
                        class="px-6 py-4 font-medium text-gray-900 whitespace-nowrap bg-gray-50 dark:text-white dark:bg-gray-800">
                        <div class="flex items-center gap-2">
                          <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="currentColor" class="w-6 h-6">
                            <path fill-rule="evenodd"
                              d="M4.5 3.75a3 3 0 0 0-3 3v10.5a3 3 0 0 0 3 3h15a3 3 0 0 0 3-3V6.75a3 3 0 0 0-3-3h-15Zm4.125 3a2.25 2.25 0 1 0 0 4.5 2.25 2.25 0 0 0 0-4.5Zm-3.873 8.703a4.126 4.126 0 0 1 7.746 0 .75.75 0 0 1-.351.92 7.47 7.47 0 0 1-3.522.877 7.47 7.47 0 0 1-3.522-.877.75.75 0 0 1-.351-.92ZM15 8.25a.75.75 0 0 0 0 1.5h3.75a.75.75 0 0 0 0-1.5H15ZM14.25 12a.75.75 0 0 1 .75-.75h3.75a.75.75 0 0 1 0 1.5H15a.75.75 0 0 1-.75-.75Zm.75 2.25a.75.75 0 0 0 0 1.5h3.75a.75.75 0 0 0 0-1.5H15Z"
                              clip-rule="evenodd" />
                          </svg>
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
                          <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="currentColor" class="w-6 h-6">
                            <path
                              d="M5.566 4.657A4.505 4.505 0 0 1 6.75 4.5h10.5c.41 0 .806.055 1.183.157A3 3 0 0 0 15.75 3h-7.5a3 3 0 0 0-2.684 1.657ZM2.25 12a3 3 0 0 1 3-3h13.5a3 3 0 0 1 3 3v6a3 3 0 0 1-3 3H5.25a3 3 0 0 1-3-3v-6ZM5.25 7.5c-.41 0-.806.055-1.184.157A3 3 0 0 1 6.75 6h10.5a3 3 0 0 1 2.683 1.657A4.505 4.505 0 0 0 18.75 7.5H5.25Z" />
                          </svg>
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
                          <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="currentColor" class="w-6 h-6">
                            <path fill-rule="evenodd"
                              d="m11.54 22.351.07.04.028.016a.76.76 0 0 0 .723 0l.028-.015.071-.041a16.975 16.975 0 0 0 1.144-.742 19.58 19.58 0 0 0 2.683-2.282c1.944-1.99 3.963-4.98 3.963-8.827a8.25 8.25 0 0 0-16.5 0c0 3.846 2.02 6.837 3.963 8.827a19.58 19.58 0 0 0 2.682 2.282 16.975 16.975 0 0 0 1.145.742ZM12 13.5a3 3 0 1 0 0-6 3 3 0 0 0 0 6Z"
                              clip-rule="evenodd" />
                          </svg>
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

                    <!-- capital -->
                    <tr class="border-b border-gray-200 dark:border-gray-700">
                      <th scope="row"
                        class="px-6 py-4 font-medium text-gray-900 whitespace-nowrap bg-gray-50 dark:text-white dark:bg-gray-800">
                        <div class="flex items-center gap-2">
                          <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="currentColor" class="w-6 h-6">
                            <path
                              d="M11.584 2.376a.75.75 0 0 1 .832 0l9 6a.75.75 0 1 1-.832 1.248L12 3.901 3.416 9.624a.75.75 0 0 1-.832-1.248l9-6Z" />
                            <path fill-rule="evenodd"
                              d="M20.25 10.332v9.918H21a.75.75 0 0 1 0 1.5H3a.75.75 0 0 1 0-1.5h.75v-9.918a.75.75 0 0 1 .634-.74A49.109 49.109 0 0 1 12 9c2.59 0 5.134.202 7.616.592a.75.75 0 0 1 .634.74Zm-7.5 2.418a.75.75 0 0 0-1.5 0v6.75a.75.75 0 0 0 1.5 0v-6.75Zm3-.75a.75.75 0 0 1 .75.75v6.75a.75.75 0 0 1-1.5 0v-6.75a.75.75 0 0 1 .75-.75ZM9 12.75a.75.75 0 0 0-1.5 0v6.75a.75.75 0 0 0 1.5 0v-6.75Z"
                              clip-rule="evenodd" />
                            <path d="M12 7.875a1.125 1.125 0 1 0 0-2.25 1.125 1.125 0 0 0 0 2.25Z" />
                          </svg>
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
                          <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="currentColor" class="w-6 h-6">
                            <path
                              d="M4.5 6.375a4.125 4.125 0 1 1 8.25 0 4.125 4.125 0 0 1-8.25 0ZM14.25 8.625a3.375 3.375 0 1 1 6.75 0 3.375 3.375 0 0 1-6.75 0ZM1.5 19.125a7.125 7.125 0 0 1 14.25 0v.003l-.001.119a.75.75 0 0 1-.363.63 13.067 13.067 0 0 1-6.761 1.873c-2.472 0-4.786-.684-6.76-1.873a.75.75 0 0 1-.364-.63l-.001-.122ZM17.25 19.128l-.001.144a2.25 2.25 0 0 1-.233.96 10.088 10.088 0 0 0 5.06-1.01.75.75 0 0 0 .42-.643 4.875 4.875 0 0 0-6.957-4.611 8.586 8.586 0 0 1 1.71 5.157v.003Z" />
                          </svg>
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
                          <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="currentColor" class="w-6 h-6">
                            <path fill-rule="evenodd"
                              d="M9 2.25a.75.75 0 0 1 .75.75v1.506a49.384 49.384 0 0 1 5.343.371.75.75 0 1 1-.186 1.489c-.66-.083-1.323-.151-1.99-.206a18.67 18.67 0 0 1-2.97 6.323c.318.384.65.753 1 1.107a.75.75 0 0 1-1.07 1.052A18.902 18.902 0 0 1 9 13.687a18.823 18.823 0 0 1-5.656 4.482.75.75 0 0 1-.688-1.333 17.323 17.323 0 0 0 5.396-4.353A18.72 18.72 0 0 1 5.89 8.598a.75.75 0 0 1 1.388-.568A17.21 17.21 0 0 0 9 11.224a17.168 17.168 0 0 0 2.391-5.165 48.04 48.04 0 0 0-8.298.307.75.75 0 0 1-.186-1.489 49.159 49.159 0 0 1 5.343-.371V3A.75.75 0 0 1 9 2.25ZM15.75 9a.75.75 0 0 1 .68.433l5.25 11.25a.75.75 0 1 1-1.36.634l-1.198-2.567h-6.744l-1.198 2.567a.75.75 0 0 1-1.36-.634l5.25-11.25A.75.75 0 0 1 15.75 9Zm-2.672 8.25h5.344l-2.672-5.726-2.672 5.726Z"
                              clip-rule="evenodd" />
                          </svg>
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
                          <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="currentColor" class="w-6 h-6">
                            <path
                              d="M15.75 8.25a.75.75 0 0 1 .75.75c0 1.12-.492 2.126-1.27 2.812a.75.75 0 1 1-.992-1.124A2.243 2.243 0 0 0 15 9a.75.75 0 0 1 .75-.75Z" />
                            <path fill-rule="evenodd"
                              d="M12 2.25c-5.385 0-9.75 4.365-9.75 9.75s4.365 9.75 9.75 9.75 9.75-4.365 9.75-9.75S17.385 2.25 12 2.25ZM4.575 15.6a8.25 8.25 0 0 0 9.348 4.425 1.966 1.966 0 0 0-1.84-1.275.983.983 0 0 1-.97-.822l-.073-.437c-.094-.565.25-1.11.8-1.267l.99-.282c.427-.123.783-.418.982-.816l.036-.073a1.453 1.453 0 0 1 2.328-.377L16.5 15h.628a2.25 2.25 0 0 1 1.983 1.186 8.25 8.25 0 0 0-6.345-12.4c.044.262.18.503.389.676l1.068.89c.442.369.535 1.01.216 1.49l-.51.766a2.25 2.25 0 0 1-1.161.886l-.143.048a1.107 1.107 0 0 0-.57 1.664c.369.555.169 1.307-.427 1.605L9 13.125l.423 1.059a.956.956 0 0 1-1.652.928l-.679-.906a1.125 1.125 0 0 0-1.906.172L4.575 15.6Z"
                              clip-rule="evenodd" />
                          </svg>
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
                          <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="currentColor" class="w-6 h-6">
                            <path fill-rule="evenodd"
                              d="M12 2.25c-5.385 0-9.75 4.365-9.75 9.75s4.365 9.75 9.75 9.75 9.75-4.365 9.75-9.75S17.385 2.25 12 2.25ZM12.75 6a.75.75 0 0 0-1.5 0v6c0 .414.336.75.75.75h4.5a.75.75 0 0 0 0-1.5h-3.75V6Z"
                              clip-rule="evenodd" />
                          </svg>
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

                  </tbody>
                </table>
              </div>
            </div>
          </div>
          <!-- Modal footer -->

        </div>
      </div>
    </div>
  </div>
</template>

