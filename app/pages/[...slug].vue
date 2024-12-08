<script setup lang="ts">
import type { NavItem } from '@nuxt/content'
import { withoutTrailingSlash } from 'ufo'

const navigation = inject<Ref<NavItem[]>>('navigation', ref([]))
const links = computed(() => navigation.value.filter(item => !['/blog', '/'].includes(item._path)) ?? [])
const route = useRoute()

const { data: page } = await useAsyncData(route.path, () => queryContent(route.path).findOne())
if (!page.value) {
  throw createError({ statusCode: 404, statusMessage: 'Page not found', fatal: true })
}

const { data: surround } = await useAsyncData(`${route.path}-surround`, () => queryContent('/')
  .where({ _extension: 'md', navigation: { $ne: false }, _dir: { $ne: 'blog' } })
  .only(['title', 'description', '_path'])
  .findSurround(withoutTrailingSlash(route.path))
, { default: () => [] })

useSeoMeta({
  title: page.value.title,
  ogTitle: page.value.title,
  description: page.value.description,
  ogDescription: page.value.description
})

defineOgImageComponent('Saas')

const headline = computed(() => findPageHeadline(page.value!))
</script>

<template>
  <UContainer>
    <UPage>
      <template #left>
        <UAside>
          <UNavigationTree :links="mapContentNavigation(links)" />
        </UAside>
      </template>

      <UPage v-if="page">
        <UPageHeader
          :title="page.title"
          :description="page.description"
          :links="page.links"
          :headline="headline"
        />

        <UPageBody prose>
          <ContentRenderer
            v-if="page.body"
            :value="page"
          />

          <UAlert
            icon="heroicons:exclamation-triangle-16-solid"
            variant="solid"
            color="primary"
            description="This information is for educational purposes and should not be considered medical advice. Always consult with a healthcare professional for any health concerns or before making any decisions related to your health or treatment."
          />

          <hr v-if="surround?.length">

          <UContentSurround :surround="surround" />
        </UPageBody>

        <template
          v-if="page.toc !== false"
          #right
        >
          <UContentToc :links="page.body?.toc?.links" />
        </template>
      </UPage>
    </UPage>
  </UContainer>

</template>
