<template>
  <nav class="navbar navbar-dark bg-secondary">
    <router-link v-if="isAuthenticated" class="navbar-brand" to="/">
      <img src="../assets/logo.png" width="30" height="30" alt="logo" />
    </router-link>
    <a v-else class="navbar-brand" href="#">
      <img src="../assets/logo.png" width="30" height="30" alt="logo"/>
    </a>
    <div>
      <div v-if="!isAuthenticated">
        <router-link to="/signin" tag="button" class="btn btn-primary mr-2">Sign In</router-link>
        <router-link to="/signup" tag="button" class="btn btn-primary">Sign Up</router-link>
      </div>
      <div v-else>
        <span class="navbar-text text-white mr-2"> {{ email }} </span>
        <button type="button" class="btn btn-danger" @click="logout" >Logout</button>
      </div>
    </div>
  </nav>
</template>

<script>
import { mapGetters, mapActions } from 'vuex'

export default {
  name: "Header",
  computed: {
      ...mapGetters(["email", "isAuthenticated"])
  },
  methods: {
      ...mapActions(["logout"])
  }
}
</script>

<style>

</style>