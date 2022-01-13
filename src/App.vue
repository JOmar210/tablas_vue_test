<template>
  <v-app>
    <v-main class="main">
      <Header/>
      <router-view/>
    </v-main>
  </v-app>
</template>

<script>
import Header from './components/Header.vue'
export default {
  name: 'App',
components: { Header},
  data: () => ({
    //
  }),
};
</script>
<style>
#app, .main{
  background-color: rgb(198, 182, 131);
}
</style>