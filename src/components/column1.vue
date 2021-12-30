<template>
  <div class="column">
    <input type="radio" id="one" value="1" v-model="pickedChild">
    <label for="one">Все</label>
    <br>
    <input type="radio" id="two" value="2" v-model="pickedChild">
    <label for="two">Меньше 0</label>
    <br>
    <input type="radio" id="three" value="3" v-model="pickedChild">
    <label for="three">Больше или равно 0</label>
    <br>
    <br>

    <button v-on:click="onClick('backward')">Назад</button>
    <button v-on:click="onClick('forward')">Вперед</button>
    <br>
    <br>

    <select v-model="ChangeSelect">
      <option v-for="option in options" :value="option.idx" v-bind:key="option.idx">
        {{ option.text }}
      </option>
    </select>

  </div>
</template>

<script>
import { computed, reactive } from "vue";

export default {
  name: "column1",
  props:{
    checkedItems: String,
    //sortedDirect: String,
    //sortedField: String
  },
  setup(props, context)
  {
    let state = reactive({
      sortedField: '',
      sortedDirect: '',
      ChangeSelect: 0,
    })
    const options= [
      { field: 'id', direct: 'forward', text: 'СортВперед по id', idx: 0 },
      { field: 'id', direct: 'backward', text: 'СортНазад по id', idx: 1 },
      { field: 'amount', direct: 'forward', text: 'СортВперед по amount', idx: 2 },
      { field: 'amount', direct: 'backward', text: 'СортНазад по amount ', idx: 3 }
    ]

    const pickedChild = computed({
      get: () => props.checkedItems,
      set: pickedChild => context.emit('update:checkedItems', pickedChild)
      })

      const onClick = (sortedDirect) => {
        state.sortedDirect = sortedDirect
        state = directServ(state, options)
        context.emit('update:sortedField', state.sortedField)
        context.emit('update:sortedDirect', state.sortedDirect)
      }

    const ChangeSelect = computed({
      get: () => state.ChangeSelect,
      set(ChangeSelect){
        state.ChangeSelect = ChangeSelect
        context.emit('update:sortedField', options[ChangeSelect].field)
        context.emit('update:sortedDirect', options[ChangeSelect].direct)
    }
    })

    return {
      pickedChild, state, options,
      onClick, ChangeSelect
    }
  }
}

const directServ = (state, options) => {
  if (state.sortedDirect === 'forward'){
    if (options.length-1 === state.ChangeSelect) state.ChangeSelect =0
    else state.ChangeSelect +=1}
  else {
    if (state.ChangeSelect === 0) state.ChangeSelect = options.length-1
    else state.ChangeSelect -=1}
  state.sortedField = options[state.ChangeSelect].field
  state.sortedDirect = options[state.ChangeSelect].direct
  return state
}


</script>

<style scoped>

</style>