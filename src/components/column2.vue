<template>
  <div class="column">
    <label v-for="item in SortedFilteredData" v-bind:key="item.id">
      <input type="checkbox" v-bind:value="item" v-model="selectedAmountsChild">
      {{ item }}
      <button v-on:click="item.amount +=1; rndData.splice(item.id-1,1,item)">+1</button>
      <button v-on:click="item.amount -=1">-1</button>
      <br>
    </label>
  </div>
</template>

<script>
import { computed, reactive } from "vue";

export default {
  name: "column2",
  props: {
    checkedItems: String,
    isSorted: Boolean,
    selectedAmounts: Array,
    sortedDirect: String,
    sortedField: String
  },
  setup(props, context) {

    const rndData = reactive([]);
    for (let i = 0; i < 100; i++) {
      rndData.push({ id: i + 1, amount: Math.floor(Math.random() * (1000 - (-1000) + 1)) + (-1000) })
    }
    const SortedFilteredData = computed(() =>
      SortedFilteredDataServ(props.sortedField, props.sortedDirect, props.checkedItems, rndData))

   const selectedAmountsChild = computed( {
      get() {
        return props.selectedAmounts
      },
      set(selectedAmountsChild) {
        context.emit('update:selectedAmounts', selectedAmountsChild)
      }
    })

    return{
      selectedAmountsChild,
      rndData,
      SortedFilteredData
    }
  }
}

const compareBy = (key) => (a, b) => a[key] - b[key]

const SortedFilteredDataServ = (sortedField, sortedDirect, checkedItems, rndData ) => {
  const FilteredData = FilteredDataServ(checkedItems, rndData)
  FilteredData.sort(compareBy(sortedField))
  if (sortedDirect === 'backward') FilteredData.reverse()
  return FilteredData
};

const FilteredDataServ = (checkedItems ,rndData) => {
  //const FilteredData = [...rndData]
  if (checkedItems === "2") return rndData.filter(function(item) {return item.amount < 0})
  else if (checkedItems === "3") return rndData.filter(function(item) {return item.amount >= 0})
  return rndData
}

</script>

<style scoped>
</style>