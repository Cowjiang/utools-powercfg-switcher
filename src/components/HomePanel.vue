<template>
  <v-container class="fill-height">
    <v-responsive class="align-center text-center fill-height">
      <v-img height="35vh" max-height="180" src="/logo.png" />

      <h1 class="mt-10 text-h4 font-weight-bold">电源计划切换</h1>
      <div class="mt-2 text-body-1 font-weight-light mb-n1">利用可用的硬件自动干衡功耗与性能</div>

      <div class="py-8" />

      <v-row class="d-flex align-center justify-center">
        <v-col cols="auto">
          <v-btn icon="mdi-cog" variant="tonal" />
        </v-col>

        <v-col cols="auto">
          <v-btn
            variant="text"
            size="large"
          >
            <v-icon
              icon="mdi-view-dashboard"
              size="large"
              start
            />
            平衡
          </v-btn>
        </v-col>

        <v-col cols="auto">
          <v-btn
            color="primary"
            size="large"
            variant="flat"
          >
            <v-icon
              icon="mdi-speedometer"
              size="large"
              start
            />
            卓越性能
          </v-btn>
        </v-col>

        <v-col cols="auto">
          <v-btn
            size="large"
            variant="text"
          >
            <v-icon
              icon="mdi-account-group"
              size="large"
              start
            />
            高性能
          </v-btn>
        </v-col>

        <v-col cols="auto">
          <v-btn
            size="large"
            target="_blank"
            variant="text"
          >
            <v-icon
              icon="mdi-leaf"
              size="large"
              start
            />
            节电
          </v-btn>
        </v-col>

        <v-col cols="auto">
          <v-btn icon="mdi-theme-light-dark" variant="tonal" @click="toggleTheme" />
        </v-col>
      </v-row>

      <div class="py-4"></div>
    </v-responsive>
  </v-container>
</template>

<script setup lang="ts">
  import { useTheme } from 'vuetify'

  const theme = useTheme()
  const toggleTheme = () => theme.global.name.value = theme.global.current.value.dark ? 'light' : 'dark'

  window?.exec && window.exec('chcp 65001 & powercfg -l', (error, stdout, stderr) => {
    if (error) {
      console.error(error);
      return;
    }
    console.log(stdout);
  });
</script>
