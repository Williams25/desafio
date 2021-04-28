<template>
  <div>
    <v-select
      :items="filterOptions"
      :label="label"
      :menu-props="{ bottom: true, offsetY: true }"
      v-model="valueModel"
      multiple
      deletable-chips
      attach
      chips
    ></v-select>
  </div>
</template>

<script>
export default {
  name: "Select",
  props: {
    filterOptions: {},
    label: String,
  },
  data: () => ({
    valueModel: "",
  }),
};
</script>

<style>
</style>