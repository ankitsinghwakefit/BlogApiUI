<template>
  <div class="container">
    {{username}}
  </div>
</template>

<script>
import axios from 'axios';
  export default {
      props: ['username'],
    data() {
      return {
        form: {
          email: '',
          password: ''
          // food: null,
          //checked: []
        },
        // foods: [{ text: 'Select One', value: null }, 'Carrots', 'Beans', 'Tomatoes', 'Corn'],
        show: true,
        token: '',
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
             this.$router.push({
              name: "Deshboard",
              });
            console.log("response after update call", response)
            // this.showContainerToUpdate = false;
          }
          // this.getAllAddedSections();
          // this.$router.go()
      }).catch(err => {
        console.log("err no response")
          throw err;
        });
        //alert(JSON.stringify(this.form))
      },
      onReset(evt) {
        evt.preventDefault()
        // Reset our form values
        this.form.email = ''
        this.form.username = ''
        // this.form.food = null
        // this.form.checked = []
        // Trick to reset/clear native browser form validation state
        this.show = false
        this.$nextTick(() => {
          this.show = true
        })
      }
    },

  mounted() {
      if(sessionStorage.getItem("token")){
          this.token = sessionStorage.getItem("token");
      }
      
      console.log("token in deshborad",this.token);
    axios({
      url: `https://shipthis.herokuapp.com/api/v1/user`,
        headers: {
            authorization: this.token,
        },
      method: "GET"
    })
      .then(response => {
          console.log("response of mounted",response);
      })
      .catch(err => {
          console.log(err);
      })
  }
  }
</script>
<style scoped>
.container {
  border: 1px solid black;
  width: 500px;
}
</style>>



