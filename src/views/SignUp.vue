<template>
  <div class="container h-100 mt-5">
    <div class="row mt-5">
      <div class="col md-6 m-auto">
        <img src="../assets/4.svg" alt="signup-svg" class="img-fluid hero-img animate__animated animate__fadeIn animate__slow"/>
      </div>

      <div class="col-md-6 m-auto">
        <form @submit.prevent="signUp" class="text-white bg-dark card card-body">
          <p class="text-center text-white text-header">
            SignUp to Get Started
          </p>

          <div class="form-group">
            <label for="">Email address</label>
            <input type="email" class="form-control" placeholder="Enter email" v-model.trim="email"/>
          </div>

          <div class="form-group">
            <label for="">Password</label>
            <input type="password" class="form-control" id="exampleInputPassword1" placeholder="Password" v-model="password"/>
            <small class="form-text text-muted">- Can be alpha-numeric</small>
            <small class="form-text text-muted">- Length should be atleast 6</small>
          </div>

          <button type="submit" class="btn btn-primary mb-2">Sign Up</button>

          <p>Already have an account? <router-link to="/signin" class="mt-2">Sign In here</router-link></p>
        </form>
      </div>
    </div>
  </div>
</template>

<script>
import firebase from "firebase/app"
export default {
    data: function(){
        return {
            email: "",
            password: ""
        }
    },
    methods: {
        signUp: function(){
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
                .createUserWithEmailAndPassword(this.email, this.password)
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
                      title: 'Sign Up was Successful'
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
}
</script>

<style scoped>
.hero-img {
  height: 445px;
  width: 100%;
}
.text-header {
  text-transform: uppercase;
  border-bottom: 1px solid blue;
  font-size: 22px;
}
</style>