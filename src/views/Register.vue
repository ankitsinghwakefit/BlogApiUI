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

      <b-form-group id="input-group-2" label="Your Name:" label-for="input-2">
        <b-form-input
          id="input-2"
          v-model="form.username"
          required
          placeholder="Enter name"
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

      <b-button type="submit" variant="primary">Register</b-button>
      <b-button type="reset" variant="danger">Reset</b-button>
    </b-form>
  </div>
</template>

<script>
import axios from 'axios';
  export default {
    data() {
      return {
        form: {
          email: '',
          username: '',
          password: ''
        },
        show: true
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
          `https://shipthis.herokuapp.com/api/v1/users`,
        method: "POST",
        data: userData
      }).then(response => {
          if (response) {
             this.$router.push({
              name: "About",
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


