<template>
  <div id="app">
    <router-view></router-view>

    <v-footer  v-show="$route.meta.showFooter"></v-footer>
  </div>
</template>

<script>

  import FooterGuider from './components/FooterGuider/FooterGuider.vue'
  import {reqFoodCategorys} from './api'
  import {mapActions} from 'vuex'

export default {
  name: 'App',
  components:{
    'v-footer':FooterGuider
  },
  mounted(){
    this.getAddress()
    this.getUserInfo()
  },
  methods: {
    ...mapActions(['getAddress', 'getUserInfo'])
  },
}
</script>

<style>

</style>
