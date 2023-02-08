<script setup>
import { nextTick, ref } from 'vue';
// import { getPercent } from './composables/button.js'
 
const histroryOperation = ref([])
// const currentCalculate = ref('0')
const lastOperation = ref('0')



// const getPercent = () => {
//   let percent = currentCalculate.value.replace(',', '.') / 100
//   currentCalculate.value = percent.toString()
// }


const allClear = () => {
  currentCalculate.value = '0'
  lastOperation.value = '0'
}

const doDelete = () => {
  if (currentCalculate.value !== '0') {
    currentCalculate.value = currentCalculate.value.toString().slice(0, -1)
  }
  if (currentCalculate.value === '') {
    currentCalculate.value = '0'
  }
}

const inputValue = (val) => {
  if (val === '0' || val === '00') {
    if (currentCalculate.value === '0') {
      currentCalculate.value = '0'
    } else {
      currentCalculate.value += maskedIn(val)
    }
  } else if (val === ',') {
    if (currentCalculate.value.includes(',')) {
      currentCalculate.value = currentCalculate.value
    } else {
      if (lastIsOperand()) {
        return currentCalculate.value
      } else {
        currentCalculate.value += maskedIn(val)
      }
    }
  } else if (val === '/' || val === '*' || val === '-' || val === '+') {
    if (lastIsOperand()) {
      let str = currentCalculate.value.toString()
      let vars = str.slice(0, -1) + val
      currentCalculate.value = maskedIn(vars)
    } else {
      currentCalculate.value += maskedIn(val)
    }
  } else {
    if (currentCalculate.value == '0' || currentCalculate.value === 'NaN') {
      currentCalculate.value = maskedIn(val)
    } else {
      currentCalculate.value += maskedIn(val)
    }
  }
  nextTick(() => {
    scrollRight()
  })
}

const scrollRight = () => {
  let scrollbar = document.getElementById('numberInput')
  scrollbar.scrollLeft = scrollbar.scrollWidth
}

const scrolldown = () => {
  let scrollbar = document.getElementById('resultElement')
  scrollbar.scrollTop = scrollbar.scrollHeight
}

const calculate = () => {
  if (lastIsOperand()) {
    let str = currentCalculate.value.toString()
    let vars = str.slice(0, -1)
    currentCalculate.value = vars
    if (checkContainsOperand(currentCalculate.value)) {
      counted()
    } else {
      return currentCalculate.value
    }
  } else {
    counted()
  }
  nextTick(() => {
    scrolldown()
  })
}
const counted = () => {
  let rep = currentCalculate.value.replace('X', '*').replace(',', '.')
  let lastCalculate = currentCalculate.value
  let currentCalt = eval(rep).toString()
  addHistroy(lastCalculate, currentCalt)
  currentCalculate.value = 0
}
const maskedIn = (val) => {
  return val.toString().replace('*', 'X')
}
const lastIsOperand = () => {
  let operand = currentCalculate.value.slice(-1)

  if (
    operand === '+' ||
    operand === '-' ||
    operand === 'X' ||
    operand === '/'
  ) {
    return true
  } else {
    return false
  }
}
const checkContainsOperand = (calculateString) => {
  const operand = ['+', '-', 'X', '/']
  let str = calculateString
  if (operand.some((v) => str.includes(v))) {
    return true
  } else {
    return false
  }
}
const addHistroy = (operation, result) => {
  if (checkContainsOperand(currentCalculate.value)) {
    histroryOperation.value.push({
      operation: operation,
      result: result
    })
  }
}
const deleteHistory = () => {
  histroryOperation.value = []
}

</script>
<template>
  <div class="h-screen w-full bg-slate-800 p-0 md:p-2">
    <div class="main-container">
      <navigator-menu :deleteHistory="deleteHistory" />
      <calculate-display :deleteHistory="deleteHistory"
                         :histroryOperation="histroryOperation"
                         :currentCalculate="currentCalculate"
                         :lastOperation="lastOperation" />
      <div class="button-wrapper">
        <div class="flex h-full w-full justify-between space-x-2 text-gray-300">
          <button-input class="btn-primary"
                        symbols="AC"
                        @click="allClear()" />
          <button-input class="btn-primary"
                        symbols="DEL"
                        @click="doDelete()" />
          <button-input class="btn-primary"
                        symbols="%"
                        @click="getPercent(currentCalculate)
                        " />
          <button-input class="btn-primary"
                        symbols="/"
                        @click="inputValue('/')" />
        </div>
        <div class="flex h-full w-full justify-between space-x-2 text-white">
          <button-input class="btn-primary"
                        symbols="7"
                        @click="inputValue(7)" />
          <button-input class="btn-primary"
                        symbols="8"
                        @click="inputValue(8)" />
          <button-input class="btn-primary"
                        symbols="9"
                        @click="inputValue(9)" />
          <button-input class="btn-primary"
                        symbols="X"
                        @click="inputValue('*')" />
        </div>
        <div class="flex h-full w-full justify-between space-x-2 text-white">
          <button-input class="btn-primary"
                        symbols="4"
                        @click="inputValue(4)" />
          <button-input class="btn-primary"
                        symbols="5"
                        @click="inputValue(5)" />
          <button-input class="btn-primary"
                        symbols="6"
                        @click="inputValue(6)" />
          <button-input class="btn-primary"
                        symbols="-"
                        @click="inputValue('-')" />
        </div>
        <div class="flex h-full w-full justify-between space-x-2 text-white">
          <button-input class="btn-primary"
                        symbols="1"
                        @click="inputValue(1)" />
          <button-input class="btn-primary"
                        symbols="2"
                        @click="inputValue(2)" />
          <button-input class="btn-primary"
                        symbols="3"
                        @click="inputValue(3)" />
          <button-input class="btn-primary"
                        symbols="+"
                        @click="inputValue('+')" />
        </div>
        <div class="flex h-full w-full justify-between space-x-2 text-white">
          <button-input class="btn-primary"
                        symbols="00"
                        @click="inputValue('00')" />
          <button-input class="btn-primary"
                        symbols="0"
                        @click="inputValue('0')" />
          <button-input class="btn-primary"
                        symbols="."
                        @click="inputValue('.')" />
          <button-input class="btn-secondary"
                        symbols="="
                        @click="calculate()" />
        </div>
      </div>
    </div>
  </div>

</template>

<style scoped>
.main-container {
  @apply m-auto w-full md:w-2/4 px-2 lg:w-1/4 rounded-none md:rounded-md ring-1 ring-blue-900 bg-slate-900 h-full flex flex-col;
}

.button-wrapper {
  @apply flex-1 py-2 flex flex-col space-y-2 text-3xl font-bold select-none;
}

.btn-primary {
  @apply bg-slate-800 hover:bg-slate-700 hover:text-blue-500;
}

.btn-secondary {
  @apply bg-blue-700 hover:bg-blue-800 hover:text-white;
}
</style>
