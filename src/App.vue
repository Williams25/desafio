<template>
  <v-app>
    <div class="container">
      <!-- <v-col cols="12">
        <v-autocomplete
          v-model="groupTag"
          :items="JSON.parse(JSON.stringify(itemsTagObj))"
          @blur="searchGroupTag()"
          dense
          label="Group Tag"
        ></v-autocomplete>

        <div @click="createOptions()">
          <v-select
            v-model="value"
            :items="JSON.parse(JSON.stringify(filterOptions))"
            label="Opções"
            :menu-props="{ bottom: true, offsetY: true }"
            multiple
            deletable-chips
            attach
            chips
          ></v-select>
        </div>
      </v-col> -->
    </div>

    <v-col cols="12">
      <Autocomplete
        :itemsTagObj="JSON.parse(JSON.stringify(itemsTagObj))"
        :searchGroupTag="searchGroupTag"
        label="Group Tag"
      />

      <div @click="createOptions()">
        <Select
          :filterOptions="JSON.parse(JSON.stringify(filterOptions))"
          label="Opções"
        />
      </div>
    </v-col>
  </v-app>
</template>

<script>
import axios from "axios";
import Autocomplete from "./components/Autocomplete";
import Select from "./components/Select";

export default {
  name: "App",
  components: {
    Autocomplete,
    Select,
  },
  data: () => ({
    list: [""],
    filters: [""],
    itemsTagObj: [""],
    itemsSelected: [""],
    filterOptions: [],
    value: [],
    groupTag: "",
  }),
  mounted() {
    this.loadingList();
  },
  methods: {
    loadingList() {
      axios
        .get("https://filters.app3.speedio.com.br/api/v3/filters.json")
        .then(async (res) => {
          const { data } = res;
          this.filters = data.filters;
          this.itemsTagObj = this.filters.map((filter) => {
            return String(filter.groupName);
          });
        })
        .catch((err) => console.error(err));
    },
    searchGroupTag() {
      this.getGroupTag();
      if (this.groupTag.length > 0) {
        this.list = this.filters.find((filter) => {
          const filtered = filter.groupName == this.groupTag;
          return filtered;
        });
        this.createItemTag();
      } else {
        this.list = [""];
      }
    },
    createItemTag() {
      if (this.groupTag.length > 0) {
        if (this.groupTag === "PORTE") {
          this.itemsSelected = this.list?.filters[0]?.filterOptions[0]?.options;
        } else {
          this.itemsSelected = this.list?.filters[0]?.filterOptions;
        }
      }
    },
    createOptions() {
      this.filterOptions = [];
      if (this.itemsSelected.length > 0 && this.groupTag.length > 0) {
        for (let i = 0; i < 20; i++) {
          if (this.itemsSelected[i].label) {
            this.filterOptions.push(this.itemsSelected[i].label);
          }
        }
      } else {
        this.itemsSelected = [];
      }
    },
    getGroupTag() {
      this.groupTag = String(sessionStorage.getItem("groupTag"));
    },
  },
};
</script>

<style scoped>
.container {
  display: flex;
  flex-direction: column;
}
.checkbox {
  margin: 2rem;
}
.input {
  display: flex;
  justify-content: center;
  align-items: center;
  margin-left: 5rem;
  margin-right: 5rem;
  width: 25rem;
}
button {
  width: 100%;
}
</style>