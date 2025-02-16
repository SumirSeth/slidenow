<template>
  <div :style="{ backgroundColor: mode==='edit'? 'gray' : bg }" class="min-h-screen">
    <nav class="fixed top-0 left-0 right-0 z-50 h-20 flex items-center justify-center">
      <div class="bg-black/40 backdrop-brightness-125 backdrop-blur-xl shadow-xl flex items-center lg:space-x-8 space-x-6 lg:px-8 px-6 py-2 rounded-2xl max-w-max">
        <div class="2xl:text-4xl xl:text-4xl lg:text-3xl md:text-3xl sm:text-2xl text-2xl font-[Stardom] font-bold text-white"><NuxtLink to="/">SlideNow</NuxtLink></div>
        <ul class="flex lg:space-x-6 space-x-4 text-white items-center 2xl:text-lg xl:text-lg lg:text-base md:text-base sm:text-sm text-sm font-[Satoshi] font-bold">
          <li><button class="flex flex-row items-center gap-1 p-1 px-3 rounded-lg hover:bg-white/10" @click="mode='view'" :class="{'border border-dashed': mode==='view'}">
            <Icon name="hugeicons:presentation-01" class="text-2xl" />
            View
          </button></li>
          <li><button class="flex flex-row items-center gap-1 p-1 px-3 rounded-lg hover:bg-white/10" @click="mode='edit'" :class="{'border border-dashed': mode==='edit'}">
            <Icon name="line-md:pencil-alt-twotone" class="text-2xl" />
            Edit
          </button></li>
        </ul>
      </div>
    </nav>
  </div>
</template>

<script lang="ts" setup>
const bg = ref("red")
const mode = ref<"edit" | "view">("edit")

</script>

<style>

</style>