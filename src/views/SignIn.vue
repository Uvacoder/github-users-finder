<template>
  <div class="container h-100">
    <div class="row mt-5">
      <div class="col-md-6">
        <img src="../assets/3.svg" alt="signin-svg" class="img-fluid hero-img animate__animated animate__fadeIn animate__slow"/>
      </div>

      <div class="col-md-6 m-auto">
        <form @submit.prevent="signIn" class="text-white bg-dark card card-body">
          <p class="text-center text-white text-header">Login to Continue</p>
          <div class="form-group">
            <label for="">Email address</label>
            <input type="email" class="form-control" id="" aria-describedby="emailHelp" placeholder="Enter email" v-model.trim = "email"/>
          </div>
          <div class="form-group">
            <label for="exampleInputPassword1">Password</label>
            <input type="password" class="form-control" id="exampleInputPassword1" placeholder="Password" v-model="password"/>
          </div>

          <button type="submit" class="btn btn-primary btn-block mb-2">Sign In</button>

          <p>Don't have an account? <router-link to="/signup" class="mt-2">Sign Up here</router-link></p>
        </form>
      </div>
    </div>
  </div>
</template>

<script>
import firebase from "firebase/app"

import { mapGetters } from 'vuex'
export default {
  
  data: function(){
      return{
          email: "",
          password: "",
      }
  },
  computed: {
      ...mapGetters(["isAuthenticated"])
  },
  watch: {
      isAuthenticated: function(newVal){
          console.log({newVal})
          if (newVal) {
              return this.$router.replace("/")
          }
      }
  },
  methods: {
      signIn: function(){
            if(!this.email && !this.password) return this.$swal({
              icon: "info",
              title:"Required!",
              text: "Please provide both Email address and Password, to continue further"
            })
            if(!this.email) return this.$swal({
              icon: "info",
              title:"Required!",
              text: "Please provide Email address"
            })
            if(!this.password) return this.$swal({
              icon: "info",
              title:"Required!",
              text: "Please provide Password"
            })

            firebase
                .auth()
                .signInWithEmailAndPassword(this.email, this.password)
                .then((res) => {
                    console.log(res)
                    this.$router.push({path: "/"})
                    this.$swal.fire({
                      toast: true,
                      position: 'top-end',
                      showConfirmButton: false,
                      timer: 3000,
                      timerProgressBar: true,
                      icon: 'success',
                      title: 'Hola! Welcome Back'
                    })
                })
                .catch((err) => {
                    this.$swal({
                        icon: "error",
                        title: "Oops!",
                        text: err.message
                    })
                })
        }
  }
  
};
</script>

<style scoped>
.hero-img {
  height: 400px;
  width: 100%;
}
.text-header {
  text-transform: uppercase;
  border-bottom: 1px solid blue;
  font-size: 22px;
}
</style>