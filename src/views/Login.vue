<template>
  <div class="container">
    <b-form @submit="onSubmit" @reset="onReset" v-if="show">
      <b-form-group
        id="input-group-1"
        label="Email address:"
        label-for="input-1"
        description=""
      >
        <b-form-input
          id="input-1"
          v-model="form.email"
          type="email"
          required
          placeholder="Enter email"
        ></b-form-input>
      </b-form-group>

      <b-form-group id="input-group-3" label="Your Password:" label-for="input-3">
        <b-form-input
          id="input-3"
          v-model="form.password"
          required
          placeholder="Enter password"
        ></b-form-input>
      </b-form-group>

      <b-button type="submit" variant="primary">Login</b-button>
      <b-button type="reset" variant="danger">Reset</b-button>
    </b-form>
    <div v-if="showLogin">
      <Deshboard :token="this.token" />
    </div>
  </div>
</template>

<script>
import axios from 'axios';
import Deshboard from './Deshboard';
  export default {
     components: {
      Deshboard
    },
    data() {
      return {
        form: {
          email: '',
          password: ''
        },
        show: true,
        showLogin: false,
        token: ''
      }
    },
    methods: {
      onSubmit(evt) {
        evt.preventDefault();
        let userData = {
          user : {...this.form}
        }
        console.log("data sent", userData);
        axios({
        url:
          `https://shipthis.herokuapp.com/api/v1/users/login`,
        method: "POST",
        data: userData
      }).then(response => {
          if (response) {
             
            this.show = false;
            this.token = response.data.user.token;
            sessionStorage.setItem("isLoggedIn", true);
            sessionStorage.setItem("token", this.token);
            this.$router.push({
              name: "Deshboard",
              });
            
          }
      }).catch(err => {
        console.log("err no response")
          throw err;
        });
      },
      onReset(evt) {
        evt.preventDefault()
        this.form.email = ''
        this.form.username = ''
        this.show = false
        this.$nextTick(() => {
          this.show = true
        })
      }
    }
  }
</script>
<style scoped>
.container {
  width: 800px;
}
</style>>



