<script lang="ts" setup>
import { AppConfigInput } from '@nuxt/schema'
import { AppSetup } from './utils/app'
import { ITheme } from './utils/theme'
AppSetup()
const theme = useState<ITheme>('theme.current')
const locale = useState<string>('locale.setting')
const app = useAppConfig() as AppConfigInput

useHead({
  title: app.name,
  titleTemplate: '%s - Erganism co-creative marketplace',
  meta: [
    { name: 'viewport', content: 'width=device-width, initial-scale=1' },
    {
      hid: 'description',
      name: 'description',
      content: 'Erganism',
    },
  ],
  link: [
    { rel: 'icon', type: 'image/x-icon', href: '/favicon.ico' },
    {
      href: '/logo/apple-touch-icon.png',
      type: 'image/x-icon',
      rel: 'apple-touch-icon',
    },
    {
      href: '/logo/android-chrome-192x192.png',
      type: 'image/x-icon',
      rel: 'android-chrome-icon',
    },
    {
      href: '/logo/android-chrome-512x512.png',
      type: 'image/x-icon',
      rel: 'android-chrome-icon',
    },
    {
      href: '/logo/logo-32x32.png',
      type: 'image/x-icon',
    },
    {
      href: '/logo/logo-16x16.png',
      type: 'image/x-icon',
    },
  ],
})
</script>

<template>
  <Html :class="`${theme === 'dark' ? 'dark' : ''}`" lang="en">
    <Body
      class="antialiased duration-300 transition-colors text-gray-800 dark:text-gray-200 bg-white dark:bg-gray-900 overscroll-y-none"
    >
      <NuxtLayout>
        <NuxtLoadingIndicator :height="5" :duration="3000" :throttle="400" />
        <NuxtPage />
      </NuxtLayout>
    </Body>
  </Html>
</template>
