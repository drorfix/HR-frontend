<template>
  <div class="layout-padding">
    <div v-if="!authenticated">
      <div class="column items-center">
        <div><img src="~assets/quasar-logo-full.svg"></div>
        <div>
          <h3>
            </br>
            <router-link to="/signin">Sign In</router-link> or <router-link to="/register">Register</router-link>
            </br></br>
          </h3>
        </div>
        <div><img src="~assets/feathers-logo.png"></div>
      </div>
    </div>
    <div v-else class="column items-center">
    </div>
  </div>
</template>

<script>

export default {
  props: ['user'],
  data () {
    return {
    }
  },
  computed: {
    authenticated () {
      return this.user !== null
    }
  },
  methods: {
  },
  mounted () {
  },
  beforeDestroy () {
  }
}
</script>

<style lang="styl">

</style>
