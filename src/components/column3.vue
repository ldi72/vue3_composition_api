<template>
  <div class="column">
    <label v-for="item in selectedAmounts" v-bind:key="item.id">
      id: {{ item.id }}, amount: {{ item.amount }}
      <br>
    </label>
  </div>
</template>

<script>
export default {
  name: "column3",
  props: {
    selectedAmounts: Array
  }
};
</script>

<style scoped>

</style>