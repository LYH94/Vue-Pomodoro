<template lang="pug">
  #app
    b-navbar#navbar(toggleable='lg' type='dark' variant='danger')
      b-navbar-nav
        b-nav-item(to='/')
          font-awesome-icon(class="icon" :icon="['far', 'clock']" v-b-popover.hover.bottomright="'番茄鐘'")
        //- https://bootstrap-vue.org/docs/components/nav#comp-ref-b-nav-item
        b-nav-item(to='/list' active-class="active")
          font-awesome-icon(class="icon" :icon="['fas', 'list']" v-b-popover.hover.bottomright="'待辦'")
        b-nav-item(to='/settings' active-class="active")
          font-awesome-icon(class="icon" :icon="['fas', 'cog']" v-b-popover.hover.bottomright="'設定'")
    //- 避免元件在換頁時被銷毀
    keep-alive
      router-view
</template>
