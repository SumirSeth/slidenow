<template>
  <div :style="{ backgroundColor: mode==='edit'? '#292524' : (text.length>0 && bg.length> 0?bg:'#292D2D') }" class="min-h-screen">
    <nav class="fixed top-0 left-0 right-0 z-50 h-20 flex items-center justify-center">
      <div class="bg-black/40 backdrop-brightness-125 backdrop-blur-xl shadow-xl flex items-center lg:space-x-8 space-x-6 lg:px-8 px-6 py-2 rounded-2xl max-w-max">
        <div class="2xl:text-4xl xl:text-4xl lg:text-3xl md:text-3xl sm:text-2xl text-2xl font-[Stardom] font-bold text-white"><NuxtLink to="/">SlideNow</NuxtLink></div>
        <ul class="flex lg:space-x-6 space-x-4 text-white items-center 2xl:text-lg xl:text-lg lg:text-base md:text-base sm:text-sm text-sm font-[Satoshi] font-bold">
          <li><button class="flex flex-row items-center gap-1 p-1 px-3 rounded-lg hover:bg-white/10" @click="mode='view';parseInput(text)" :class="{'border border-dashed': mode==='view'}">
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

    <div class="flex justify-center pt-20 text-white">
      <div v-if="mode === 'edit'" class="editor w-full px-6 md:px-12 lg:px-20">
        <h1 class="2xl:text-4xl xl:text-4xl lg:text-3xl md:text-3xl sm:text-2xl text-2xl font-[Stardom] font-bold p-2">Slide Editor</h1>
        <textarea 
          class="w-full h-[calc(100vh-12rem)] rounded-lg bg-stone-700 focus:outline-none p-4 resize-none font-[Satoshi] shadow-xl"
          placeholder="Start writing your slides..."
          v-model="text"
        ></textarea>
      </div>

      <div v-else class="viewer">
        <div v-if="text.length <= 0" class="no-text flex justify-center items-center h-[80svh]">
          <h1 class="2xl:text-4xl xl:text-4xl lg:text-3xl md:text-3xl sm:text-2xl text-2xl font-[Stardom] font-bold p-2"><span class="text-red-500">Err!</span> No text found. Start typing in the <span class="italic underline decoration-dotted underline-offset-8 cursor-cell" @click="mode='edit'">edit</span> page.</h1>
        </div>
      </div>

    </div>



  </div>
</template>

<script lang="ts" setup>
const bg = ref("")
const mode = ref<"edit" | "view">("edit")

const text = ref("")

const parseInput = (input: string) => {
  const sections = input.split('---').filter((section) => section.trim() !== "");
  let slideInfo = {
    color: '',
    rows: 0,
    columns: 0,
  }
  let blocks: any[] = []

  sections.forEach((section) => {
    // Split section into lines and process each line
    const lines = section.split('\n').map(line => line.trim());
    
    lines.forEach(line => {
      if (line.startsWith('color:')) {
        slideInfo.color = line.replace('color:', '').trim();
      }
      if (line.startsWith('rows:')) {
        slideInfo.rows = parseInt(line.replace('rows:', '').trim());
      }
      if (line.startsWith('columns:')) {
        slideInfo.columns = parseInt(line.replace('columns:', '').trim());
      }
    });

    // Process blocks
    if (section.includes('title:') && section.includes('description:')) {
      const title = lines.find(line => line.startsWith('title:'))?.replace('title:', '').trim() || '';
      const description = lines.find(line => line.startsWith('description:'))?.replace('description:', '').trim() || '';
      const row = parseInt(lines.find(line => line.startsWith('row:'))?.replace('row:', '').trim() || '0');
      const column = parseInt(lines.find(line => line.startsWith('column:'))?.replace('column:', '').trim() || '0');

      blocks.push({ row, column, title, description });
    }
  });

  bg.value = slideInfo.color // Update the background color ref
  console.log(slideInfo, blocks)
  return { slideInfo, blocks };
}


</script>

<style>

textarea::-webkit-scrollbar{
  width: 6px;
  cursor: pointer;
}

textarea::-webkit-scrollbar-track{
  background: #292524;
  cursor: pointer;
}

textarea::-webkit-scrollbar-thumb{
  background: #57534e;
  border-radius: 10px;
  cursor: pointer;
}

textarea::-webkit-scrollbar-thumb:hover{
  background: #79736c;
  cursor: pointer;
}


</style>