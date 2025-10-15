<template>
  <div class="bg-gray-100 flex flex-row rounded-md p-2 gap-4 border">
    <div class="flex items-center jsutify-center">
      <Icon :name="item.icon" class="w-[30px] h-[30px]"/></div>
    <div>
      <div>{{ item.name }}</div>
      <div>{{ item.description }}</div>
    </div>
  </div>
</template>

<script lang="ts" setup>
import type { TechnologiesCollectionItem } from "@nuxt/content";
defineProps<{
  item: TechnologiesCollectionItem;
}>();
</script>

<style></style>
