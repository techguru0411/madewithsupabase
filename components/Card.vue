<template>
  <NuxtLink :to="`/p/${item.slug}`" class="h-full">
    <div
      class="bg-dark-600 flex flex-col rounded-2xl h-full border-5 border-dark-300 hover:border-gray-300 hover:shadow-gray-300 hover:shadow-lg transition ease-in-out overflow-hidden"
    >
      <div class="relative flex-shrink-0 pb-2/3">
        <CompressedImage
          :alt="item.title"
          v-if="item.images[0]"
          :src="item.images[0]"
        />
        <div
          class="absolute w-full h-full flex justify-center items-center object-cover rounded-xl"
          v-else
        >
          <img
            class="w-24 h-24 opacity-50 filter grayscale"
            :src="SiteLogo"
            alt=""
          />
        </div>
      </div>
      <div
        class="p-4 sm:p-6 h-full flex flex-col justify-between border-t-5 border-dark-300"
      >
        <h1 class="text-2xl">{{ item.title }}</h1>
        <p
          class="break-words text-light-900 flex-1 my-4 leading-5 line-clamp-2"
        >
          {{ cleanse(item.description) }}
        </p>
        <div class="flex justify-between items-center">
          <div class="text-sm text-warm-gray-600 flex flex-wrap">
            <p v-for="category in trimCategories" :key="category" class="mr-2">
              #{{ category }}
            </p>
          </div>
          <div class="flex items-center flex-shrink-0 text-light-900">
            <p class="mr-1">{{ separator(item.views) }}</p>
            <div class="i-ic:twotone-ads-click"></div>
          </div>
        </div>
      </div>
    </div>
  </NuxtLink>
</template>

<script setup lang="ts">
import removeMd from "remove-markdown"
import SiteLogo from "@/assets/logo.svg"

const prop = defineProps({
  item: {
    type: Object,
    required: true,
  },
})

const trimCategories = computed(() => {
  return prop.item.categories.slice(0, 6)
})

const cleanse = (text: string) => {
  let t = text.replace(/(?:https?|ftp):\/\/[\n\S]+/g, "")
  return removeMd(t).slice(0, 100)
}

const separator = (number: number) => {
  var str = number.toString().split(".")
  str[0] = str[0].replace(/\B(?=(\d{3})+(?!\d))/g, ",")
  return str.join(".")
}
</script>
