<script setup>
defineProps({
  deleteHistory: Function
})

</script>



<template>
  <div class="flex items-center justify-between py-2 rounded-t-md">
    <div class="flex space-x-2">
      <div class="menu menu-active">
        <calculator-icon class="text-blue-500" />
        <p class="text-sm font-medium text-blue-500">计算器</p>
      </div>
      <a href="https://github.com/Z-Yun-H/calculator"
         target="_blank"
         class="menu">
        <github-icon class="" />
        <p class="text-sm font-medium">Github</p>
      </a>
    </div>
    <div class="menu">
      <trash-icon @click="deleteHistory" />
    </div>
  </div>
</template>

<style>
.menu {
  @apply flex items-center space-x-0.5 bg-slate-900 text-blue-800 hover:text-blue-500 hover:bg-slate-700 px-1.5 py-1 rounded cursor-pointer;
}

.menu-active {
  @apply bg-slate-700;
}
</style>
