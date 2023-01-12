<template>
  <div id="app">
    <custom-vue-multi-select
      v-model="selectedOption"
      placeholder="نمایش همه"
      :options="cycleDetail"
      based-on="text"
      target-key="value"
      :default-selected="[25]"
    />
  </div>
</template>

<script>
import CustomVueMultiSelect from "./components/CustomVueMultiSelect.vue";

export default {
  name: "App",
  components: {
    CustomVueMultiSelect,
  },
  data() {
    return {
      selectedOption: '',
      cycleDetail: [
        { text: "فروردین-1401", value: 13 },
        { text: "اردیبهشت-1401", value: 14 },
        { text: "خرداد-1401", value: 15 },
        { text: "تیر-1401", value: 16 },
        { text: "مرداد-1401", value: 17 },
        { text: "شهریور-1401", value: 18 },
        { text: "مهر-1401", value: 19 },
        { text: "آبان-1401", value: 20 },
        { text: "آذر-1401", value: 21 },
        { text: "دی-1401", value: 22 },
        { text: "بهمن-1401", value: 23 },
        { text: "اسفند-1401", value: 24 },
        { text: "فروردین-1402", value: 25 },
      ],
    };
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
