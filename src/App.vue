<template>
  <v-theme-provider :theme="currentTheme">
    <v-app>
      <HomePanel />
    </v-app>
  </v-theme-provider>
</template>

<script setup lang="ts">
  import { computed } from 'vue'
  import HomePanel from '@/components/HomePanel.vue'
  import { useTheme } from 'vuetify'

  const theme = useTheme()

  const currentTheme = computed(() => theme.current.value?.dark ? 'dark' : 'light')

  const init = () => {
    const localTheme = localStorage.getItem('theme') ?? ''
    localTheme && (theme.global.name.value = localTheme)
  }
  init()
</script>

<style>
  html {
    overflow: auto !important;
    cursor: default;
    user-select: none;
  }
</style>
