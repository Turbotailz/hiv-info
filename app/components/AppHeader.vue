<script setup lang="ts">
import type { NavItem } from '@nuxt/content'

const navigation = inject<Ref<NavItem[]>>('navigation', ref([]))
const children = computed(() => navigation.value.filter(item => !['/blog', '/'].includes(item._path)) ?? [])

console.log( mapContentNavigation(children.value))

const menuLinks = mapContentNavigation(children.value)
</script>

<template>
  <UHeader :links="menuLinks">
    <template #logo>
      <UIcon
        name="i-fa6-solid:ribbon"
        class="w-7 h-7 text-primary"
      />
    </template>

    <template #right>
      <UContentSearchButton
        class="rounded-md"
        size="sm"
      />
    </template>

    <template #panel>
      <UNavigationTree
        :links="menuLinks"
        default-open
      />
    </template>
  </UHeader>
</template>
