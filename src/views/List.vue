<template lang="pug">
  b-container(fluid)#list.overflow-auto
    b-row
      b-col(cols="12" lg="6" class="my-3")
        b-form-group(label="新增事項" invalid-feedback="請至少輸入兩個字" :state="state")
          b-form-input(v-model="newtodo" trim :state="state" @keydown.enter="addTodo")
        b-btn(variant="danger" @click="addTodo") 新增
      b-col(cols="12")
        b-table(:items="todos" :fields="fields")
          template(#cell(name)="data")
            b-form-input(
              v-if="data.item.edit"
              v-model="data.item.model"
              trim
              :state="stateTodo(data.item.model)"
              @keydown.enter="changeTodo(data.index)"
            )
            span(v-else) {{ data.item.name }}
          template(#cell(action)="data")
            span(v-if="data.item.edit")
              b-btn(size="sm" variant="success" @click="changeTodo(data.index)" class="mr-2")
                font-awesome-icon(:icon="['fas', 'check']")
              b-btn(size="sm" variant="danger" @click="cancelTodo(data.index)")
                font-awesome-icon(:icon="['fas', 'undo']")
            span(v-else)
              b-btn(size="sm" variant="primary" @click="editTodo(data.index)" class="mr-2")
                font-awesome-icon(:icon="['fas', 'pen']")
              b-btn(size="sm" variant="danger" @click="delTodo(data.index)")
                font-awesome-icon(:icon="['fas', 'times']")
</template>

<script>
export default {
  name: 'List',
  data () {
    return {
      newtodo: '',
      fields: [
        {
          key: 'name',
          label: '待辦事項'
        },
        {
          key: 'action',
          label: '操作'
        }
      ]
    }
  },
  computed: {
    state () {
      if (this.newtodo.length === 0) {
        return null
      } else if (this.newtodo.length < 2) {
        return false
      } else {
        return true
      }
    },
    todos () {
      return this.$store.state.todos
    }
  },
  methods: {
    addTodo () {
      if (this.state) {
        this.$store.commit('addTodo', this.newtodo)
        this.newtodo = ''
      }
    },
    delTodo (index) {
      this.$store.commit('delTodo', index)
    },
    editTodo (index) {
      this.$store.commit('editTodo', index)
    },
    stateTodo (data) {
      if (data.length < 2) {
        return false
      } else {
        return true
      }
    },
    changeTodo (index) {
      const valid = this.stateTodo(this.todos[index].model)

      if (valid) {
        this.$store.commit('changeTodo', index)
      }
    },
    cancelTodo (index) {
      this.$store.commit('cancelTodo', index)
    }
  }
}
</script>
