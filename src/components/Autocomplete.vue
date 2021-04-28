<template>
  <div>
    <v-autocomplete
      v-model="valueModel"
      :items="itemsTagObj"
      :label="label"
      @blur="groupTag()"
      dense
      filled
    ></v-autocomplete>
  </div>
</template>

<script>
export default {
  name: "Autocomplete",
  props: {
    itemsTagObj: {},
    label: String,
    searchGroupTag: Function,
  },
  data: () => ({
    valueModel: "",
  }),
  methods: {
    groupTag() {
      sessionStorage.setItem("groupTag", this.valueModel);
      this.$props.searchGroupTag()
    },
  },
};
</script>

<style>
</style>