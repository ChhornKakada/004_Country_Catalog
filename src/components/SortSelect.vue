<script>
export default {
  name: "SortSelect",
  props: {
    option_selected: String,
    options: Array
  },
  data() {
    return {
      option_selected: "def",
    };
  },
  methods: {
    changeValue() {
      this.$emit("change_option", this.option_selected);
    },
  },
};
</script>

<template>
  <div class="flex gap-2 items-center text-xl">
    <div class="flex">
      <button id="states-button" data-dropdown-toggle="dropdown-states"
        class="flex-shrink-0 z-10 inline-flex items-center py-2.5 px-4 text-sm font-medium text-center text-gray-500 bg-gray-100 border border-gray-300 rounded-s-lg hover:bg-gray-200 focus:ring-4 focus:outline-none focus:ring-gray-100 dark:bg-gray-700 dark:hover:bg-gray-600 dark:focus:ring-gray-700 dark:text-white dark:border-gray-600"
        type="button">

        <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor"
          class="w-6 h-6">
          <path stroke-linecap="round" stroke-linejoin="round" d="M3.75 6.75h16.5M3.75 12H12m-8.25 5.25h16.5" />
        </svg>
        <span class="pl-2">Sort by</span>
      </button>

      <select id="states" v-model="option_selected" @change="changeValue"
        class="bg-gray-50 border border-gray-300 text-gray-900 text-sm rounded-e-lg border-s-gray-100 dark:border-s-gray-700 border-s-2 focus:ring-blue-500 focus:border-blue-500 block w-full p-2.5 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500">
        <option v-for="opt, index in options" :key="index" :value="opt.value"> {{ opt.label }} </option>
      </select>
    </div>
  </div>
</template>

