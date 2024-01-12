<script>
export default {
  name: "RowInfo",
  props: {
    key: String,
    value: String
  }
};
</script>

<template>
  <!-- Area -->
    <th scope="row"
      class="px-6 py-4 font-medium text-gray-900 whitespace-nowrap bg-gray-50 dark:text-white dark:bg-gray-800">
      {{ key }}
    </th>
    <td class="px-6 py-4">
      {{ value }}
    </td>
  
  <!-- end Area -->
</template>