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

    <custom-vue-select
      id="month"
      v-model="selectedCycle"
      :options="cycleData"
      based-on="cycle_name"
      target-key="cycle_detail_id"
    >
      <template #option="{ item, click }">
        <div
          class="d-flex align-items-center justify-content-between"
          style="cursor: pointer"
        >
          <span @click="click">{{ item.cycle_name }}</span>
        </div>
      </template>
    </custom-vue-select>
  </div>
</template>

<script>
import CustomVueMultiSelect from "./components/CustomVueMultiSelect.vue";
import CustomVueSelect from "./components/CustomVueSelect.vue";
export default {
  name: "App",
  components: {
    CustomVueMultiSelect,
    CustomVueSelect
  },
  data() {
    return {
      selectedOption: "",
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

      cycleData: [
        { cycle_detail_id: 13, cycle_name: "فروردین-1401", is_current: 0 },
        { cycle_detail_id: 14, cycle_name: "اردیبهشت-1401", is_current: 0 },
        { cycle_detail_id: 15, cycle_name: "خرداد-1401", is_current: 0 },
        { cycle_detail_id: 16, cycle_name: "تیر-1401", is_current: 0 },
        { cycle_detail_id: 17, cycle_name: "مرداد-1401", is_current: 0 },
        { cycle_detail_id: 18, cycle_name: "شهریور-1401", is_current: 0 },
        { cycle_detail_id: 19, cycle_name: "مهر-1401", is_current: 0 },
        { cycle_detail_id: 20, cycle_name: "آبان-1401", is_current: 0 },
        { cycle_detail_id: 21, cycle_name: "آذر-1401", is_current: 0 },
        { cycle_detail_id: 22, cycle_name: "دی-1401", is_current: 1 },
        { cycle_detail_id: 23, cycle_name: "بهمن-1401", is_current: 0 },
        { cycle_detail_id: 24, cycle_name: "اسفند-1401", is_current: 0 },
        { cycle_detail_id: 25, cycle_name: "فروردین-1402", is_current: 0 },
      ],
      selectedCycle:''
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
