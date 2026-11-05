<script setup lang="ts">
import 'vue-sonner/style.css'

const { title, description, image } = useAppConfig()
const route = useRoute()
const localeHead = useLocaleHead()
const colorMode = useColorMode()

useSeoMeta({
  title: `${title} - Link Shortener with Analytics`,
  description,
  ogType: 'website',
  ogTitle: title,
  ogSiteName: title,
  ogDescription: description,
  ogImage: image,
  twitterTitle: title,
  twitterDescription: description,
  twitterImage: image,
  twitterCard: 'summary_large_image',
})

useHead(() => ({
  htmlAttrs: localeHead.value.htmlAttrs,
  meta: [
    {
      name: 'viewport',
      content: 'width=device-width, initial-scale=1, viewport-fit=cover',
    },
    {
      name: 'theme-color',
      content: colorMode.value === 'dark' ? '#09090b' : '#ffffff',
    },
  ],
  link: [
    {
      rel: 'canonical',
      href: computed(() => `https://sink.cool${route.path}`),
    },
    {
      rel: 'icon',
      type: 'image/svg+xml',
      href: '/images/favicons/favicon.svg',
    },
    // We need PNGs per theme because iOS ignores SVG favicons and a PNG only has one look
    ...(['light', 'dark'] as const).flatMap((colorScheme) => {
      const media = `(prefers-color-scheme: ${colorScheme})`
      const directory = `/images/favicons/${colorScheme}`
      return [
        { rel: 'icon', type: 'image/png', sizes: '16x16', media, href: `${directory}/favicon-16x16.png` },
        { rel: 'icon', type: 'image/png', sizes: '32x32', media, href: `${directory}/favicon-32x32.png` },
        { rel: 'apple-touch-icon', sizes: '180x180', media, href: `${directory}/apple-touch-icon.png` },
      ]
    }),
  ],
}))
</script>

<template>
  <NuxtLayout>
    <NuxtLoadingIndicator color="var(--primary)" />
    <NuxtPage />
    <Toaster position="top-center" rich-colors />
  </NuxtLayout>
</template>
