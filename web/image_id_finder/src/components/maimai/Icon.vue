<script setup lang="ts">
import ImageContent from "@/components/ImageContent.vue";
import {onMounted, ref, computed} from "vue";

const iconList = ref<{ id: number, name: string, description: string, genre: string }[]>([]);
const pages = ref(1)

async function fetchIconList() {
  const response = await fetch('https://maimai.lxns.net/api/v0/maimai/icon/list', {
    method: 'GET',
  });
  const data = await response.json()
  return data.icons;
}

onMounted(() => {
  fetchIconList().then(data => {
    iconList.value = data;
  }).catch(error => {
    console.error('Error fetching plate list:', error);
  });
})

const handlePageChange = (page: number) => {
  pages.value = page
}

const resultVal = computed(() => {
  return iconList.value.slice((pages.value - 1) * 50, pages.value * 50)
})

</script>

<template>
      <div class="w-full mb-4 flex justify-center">
        <el-pagination size="small" layout="prev, pager, next" :total="iconList.length" :default-page-size="50" @current-change="handlePageChange" />
      </div>
  <div class="img-sets flex flex-wrap justify-center gap-4">

    <div v-for="icon in resultVal" :key="icon.id">

      <ImageContent
          width="120px"
          :src="`https://assets2.lxns.net/maimai/icon/${icon.id}.png`"
          :id="icon.id.toString()"
      />
    </div>
  </div>
</template>

