<template>
  <div class="home"></div>
  <!--column1  :checked-items = "checkedItems" @update:checked-items = "checkedItems = $event"
                @update:sorted-direct = "sortedDirect = $event"
                @update:sorted-field = "sortedField = $event"
                v-model:sorted-direct = "sortedDirect"
                v-model:sorted-field = "sortedField"
  /-->
  <column1  v-model:checked-items = "checkedItems"
            @update:sorted-direct = "sortedDirect = $event"
            @update:sorted-field = "sortedField = $event"
  />
  <column2  v-model:selected-amounts = "selectedAmounts"
            v-model:checked-items = "checkedItems"
            v-model:sorted-direct = "sortedDirect"
            v-model:sorted-field = "sortedField"
  />
  <column3  v-model:selected-amounts = "selectedAmounts"/>
</template>

<script>
import column1 from '../components/column1';
import column2 from '../components/column2';
import column3 from '../components/column3';
import { ref } from "vue";

export default {
  name: "Home",
  components: {
    column1, column2, column3
  },
  setup(){
    const selectedAmounts = ref([])
    const checkedItems = ref('1')
    const sortedDirect = ref('forward')
    const sortedField = ref('id')

    return {
      selectedAmounts,
      checkedItems,
      sortedDirect,
      sortedField
    }
  }
}
</script>

<style>
html, body, #app {
  padding: 0;
  margin: 0;
  width: 100%;
  height: 100%;
}
#app {
  display: flex;
}
.column {
  border: 1px solid lightgray;
  overflow: hidden auto;
  min-width: 33%;
}
</style>