<template>
  <v-container class="fill-height">
    <v-responsive class="align-center text-center fill-height">
      <div class="py-4" />
      <v-img height="35vh" max-height="180" src="/logo.png" />

      <h1 class="mt-10 text-h4 font-weight-bold">电源计划切换</h1>
      <div class="mt-2 text-body-1 font-weight-light mb-n1">利用可用的硬件自动干衡功耗与性能</div>

      <div class="py-8" />

      <v-row class="d-flex align-center justify-center">
        <v-col
          v-for="plan in powerPlans"
          :key="plan.guid"
          cols="auto"
        >
          <v-btn
            :color="plan.selected ? 'primary' : ''"
            :variant="plan.selected ? 'flat' : 'text'"
            size="large"
            @click="setPowerConfigActive(plan?.guid)"
          >
            <v-icon
              icon="mdi-speedometer"
              size="large"
              start
            />
            {{ plan.name }}
          </v-btn>
        </v-col>

        <v-col cols="auto">
          <v-menu density="compact">
            <template v-slot:activator="{ props }">
              <v-btn
                size="large"
                target="_blank"
                variant="text"
                v-bind="props"
              >
                <v-icon
                  icon="mdi-dots-horizontal-circle"
                  size="large"
                  start
                />
                选项
              </v-btn>
            </template>

            <v-list density="compact">
              <v-list-item density="compact" @click="toggleTheme">
                切换主题
              </v-list-item>
              <v-list-item density="compact" @click="openSystemPowerConfig">
                电源计划设置
              </v-list-item>
            </v-list>
          </v-menu>
        </v-col>
      </v-row>

      <div class="py-4"></div>
    </v-responsive>
  </v-container>
</template>

<script setup lang="ts">
  import { ref } from 'vue'
  import { useTheme } from 'vuetify'

  const theme = useTheme()
  const toggleTheme = () => {
    theme.global.name.value = theme.global.current.value.dark ? 'light' : 'dark'
    localStorage.setItem('theme', theme.global.name.value)
  }

  interface PowerPlan {
    name?: string,
    guid?: string,
    icon?:string,
    selected: boolean
  }
  const powerPlans = ref<PowerPlan[]>([])

  const parseIcon=(name:string)=>{
const iconMap={
  "mdi-view-dashboard":["平衡"],
  "mdi-speedometer":["卓越","高性能"],
  "mdi-leaf":["节能","节电","节约","节"]
}

for(const key in iconMap){
  const keywords = iconMap[key];
  if (keywords.find(v => name.indexOf(v) > -1)) {
    return key;
  }
}
return "lightning-bolt"
  }
  const parsePlans = (stdout) => {
    const matches = stdout.match(/GUID.*/g)
    powerPlans.value = matches?.map(match => {
     const name= match.match(/\((\S+)\)/)[1]
      return {
        name,
        guid: match.match(/(?<=GUID:\s*)\S+/)[0],
icon:parseIcon(name),
        selected: match.indexOf('*') > -1
      }
    }) ?? []
  }

  const init = () => {
    window?.exec && window.exec('chcp 65001 & powercfg -l', (error, stdout, stderr) => {
      if (error) {
        console.error(error)
        return
      }
      parsePlans(stdout)
      console.log(powerPlans.value)
    })

  }
  init()

  // 激活电源计划
  const setPowerConfigActive = (guid?: string) => {
    if (!guid) return
    window?.exec && window.exec(`powercfg -setactive ${guid}`, (error, stdout, stderr) => {
      if (error) {
        //TODO dealWith error
        return
      }
      init()
    })
  }

  // 打开电源计划设置
  const openSystemPowerConfig = () => {
    window?.exec && window.exec('powercfg.cpl', (error, stdout, stderr) => {
      if (error) {
        console.error(error)
      }
    })
  }
</script>
