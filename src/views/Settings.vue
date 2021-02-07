<template lang="pug">
  #settings
    b-container
      b-row
        b-col(cols="12")
          b-table(:items="items" :fields="fields" @row-clicked="slectAlarm")
            template(#cell(file)="data")
              audio(controls :src="'./sounds/'+data.item.file")
            template(#cell(select)="data")
              font-awesome-icon(v-if="data.item.file === alarm" :icon="['fas', 'check']")
</template>

<script>
export default {
  name: 'Settings',
  data () {
    return {
      items: [
        {
          name: '鬧鐘',
          file: 'alarm-clock.wav'
        },
        {
          name: '外星人',
          file: 'alarm-alien.wav'
        },
        {
          name: '警報',
          file: 'alarm-clock-loop.wav'
        },
        {
          name: '鬧鈴',
          file: 'alarm-short.wav'
        },
        {
          name: '鐘聲',
          file: 'alarm-bell.wav'
        }
      ],
      fields: [
        {
          key: 'name',
          label: '名稱'
        },
        {
          key: 'file',
          label: '預覽'
        },
        {
          key: 'select',
          label: '選擇'
        }
      ]
    }
  },
  computed: {
    alarm () {
      return this.$store.state.alarm
    }
  },
  methods: {
    slectAlarm (item) {
      this.$store.commit('selectAlarm', item.file)
    }
  }
}
</script>
