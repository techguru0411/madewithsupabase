<template>
  <div>
    <CustomMeta
      :key="$route.params.name.toString()"
      :title="'Tag: ' + $route.params.name"
    />

    <transition name="fade" mode="out-in">
      <div v-if="routeData" class="mt-12">
        <div v-if="routeData.length" class="card-grid">
          <Card v-for="item in routeData" :item="item"></Card>
        </div>
        <div v-else class="w-full flex flex-col space-y-8 items-center mt-32">
          <img class="w-40" src="@/assets/404.svg" />
          <h1 class="text-4xl">No project found...</h1>
          <button
            @click="$router.push('/')"
            class="inline-flex items-center text-dark-50 hover:text-light-900 transition"
          >
            <div class="i-mdi:menu-left mr-2 w-6 h-6"></div>
            Home
          </button>
        </div>
      </div>
      <div v-else class="w-full h-screen flex items-center justify-center">
        <SVGCircle class="animate-ping"></SVGCircle>
      </div>
    </transition>
  </div>
</template>

<script setup lang="ts">
import { TagData } from "~~/script/interface"

const route = useRoute()
const name = computed(() => route.params.name.toString())
const { data: routeData } = await useLazyAsyncData<TagData[]>(name.value, () =>
  $fetch(`/api/tag?name=${name.value}`)
)
</script>
