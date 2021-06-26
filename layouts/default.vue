<template>
  <v-app dark>
    <v-app-bar
      :clipped-left="clipped"
      fixed
      app
      color="#204ecf"
    >
      <v-toolbar-title class="header-title" v-text="title" />
      <v-spacer />
    </v-app-bar>
    <v-main>
      <v-container>
        <nuxt />
      </v-container>
    </v-main>
    <v-footer
      :absolute="!fixed"
      app
      color="#262d3d"
      height="75"
    >
      <span class="header-title">© Copyright 2021 Wordsmerge.com</span>
      <v-spacer></v-spacer>
      <span class="header-title">Contact Us: Wordsmerge@inboxeen.com</span>

    </v-footer>
  </v-app>
</template>

<script>
export default {
  data () {
    return {
      clipped: false,
      fixed: false,
      title: 'Words Merge'
    }
  }
}
</script>
<style scoped>
.header-title{
  color: white;
}
</style>
