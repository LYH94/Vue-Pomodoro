<template lang="pug">
  #home
    div.home
      span
      span
      h2 {{ currentText }}
      h2 {{ timetext }}
    div.btn
      b-btn(variant="danger" v-if="status !== 1" @click="start" class="mr-2")
        font-awesome-icon(:icon="['fas', 'play']")
      b-btn(variant="danger" v-if="status === 1" @click="pause" class="mr-2")
        font-awesome-icon(:icon="['fas', 'pause']")
      b-btn(variant="danger" v-if="current.length > 0" @click="finish(true)")
        font-awesome-icon(:icon="['fas', 'step-forward']")
</template>

<script>
export default {
  name: 'Home',
  data () {
    return {
      timer: 0
    }
  },
  computed: {
    status: {
      get () {
        return this.$store.state.status
      },
      set (value) {
        this.$store.commit('changeStatus', value)
      }
    },
    current () {
      return this.$store.state.current
    },
    currentText () {
      let result = ''

      if (this.current.length > 0) {
        result = this.current
      } else if (this.todos.length > 0) {
        result = '點擊開始'
      } else {
        result = '沒有事項'
      }

      return result
    },
    timeleft () {
      return this.$store.state.timeleft
    },
    alarm () {
      return this.$store.state.alarm
    },
    todos () {
      return this.$store.state.todos
    },
    timetext () {
      const m = Math.floor(this.timeleft / 60)
      const s = Math.floor(this.timeleft % 60)
      return `${m} : ${s}`
    }
  },
  methods: {
    start () {
      // 不是暫停繼續，且待辦有東西時才把 todos 的第一個變成 current
      if (this.status !== 2 && this.todos.length > 0) {
        this.$store.commit('start')
      }
      if (this.current.length > 0) {
        this.status = 1
        this.timer = setInterval(() => {
          this.$store.commit('countdown')
          if (this.timeleft <= -1) {
            this.finish(false)
          }
        }, 1000)
      }
    },
    finish (skip) {
      clearInterval(this.timer)
      this.status = 0
      this.$store.commit('finish')

      if (!skip) {
        const audio = new Audio()
        audio.src = './sounds/' + this.alarm
        audio.play()
      }

      if (this.todos.length > 0) {
        this.start()
      } else {
        alert('全部結束')
      }
    },
    pause () {
      clearInterval(this.timer)
      this.status = 2
    }
  }
}
</script>
