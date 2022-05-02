<template>
  <div>
    <label>
      Email:
      <input :type="text" v-model="email" name="email" />
    </label>
    <label>
      Password:
      <input :type="text" v-model="password" name="password" />
    </label>

    <div class="buttons">
      <button v-on:click="login()">Login</button>
      <button v-on:click="listEvents()">ListEvents</button>

      <!-- <button @click="logout">Logout</button> -->
      <!-- <button @click="register">Register</button> -->
    </div>
  </div>
</template>
<script>
import axios from 'axios';

export default {
  data() {
    return {
      email: "",
      password: "",

    };


  },
  methods: {
    login() {
      //  axios.defaults.headers.common['Access-Control-Allow-Origin'] = '*';
      axios.post("http://127.0.0.1:8000/api-token-auth/", { username: this.email, password: this.password },
        {
          'headers': {
            'Accept': 'application/json',
            'Content-Type': 'application/json;charset=UTF-8'
          }
        })
        .then((response => {
          localStorage.setItem('auth-token', JSON.stringify(response.data))
          console.log(response.data);
          
        }))
        .catch((error) => {
          console.log(error);
        });
      // var axios = require('axios');
      // axios.defaults.headers.common['Access-Control-Allow-Origin'] = '*';
      // var config = {
      //   method: 'get',
      //   url: 'http://127.0.0.1:8000/events/deneme',
      //   headers: {
      //     'Authorization': 'Token cc4356883ee4d517145221e1d60cebaed658dc1d',
      //     'Cookie': 'csrftoken=V6Ks2t0gbwU7CSJRxwHArzuQWnnt7XEqeSYsGPL1PGS2B8Lj3PfzOuj5LZlEgG9C',
      //     'Access-Control-Allow-Origin': '*',
      //     'Connection':'keep-alive',
      //     'Accept-Encoding':'gzip, deflate, br',
      //     'Accept':'*/*',

      //   'Access-Control-Allow-Methods': 'GET, PUT, POST, DELETE, OPTIONS, post, get',
      //   'Access-Control-Max-Age':'3600',
      //   'Access-Control-Allow-Headers': 'Origin, Content-Type, X-Auth-Token',
      //   'Access-Control-Allow-Credentials': 'true',

      //   }
      // };

      // let result = axios(config)
      //   .then(function (response) {
      //     console.log(JSON.stringify(response.data));
      //   })
      //   .catch(function (error) {
      //     console.log(error);
      //   });


    },

    listEvents() {
      // axios.interceptors.request.use(
      //   config => {

      //     //here retrieve the token and add it (if present)
      //     //most probably you would use VUEX for storing it 
      //     //in you app 
      //     const token = localStorage.getItem('auth-token');
      //     console.log(token,)
      //     if (token) {
      //       config.headers['Authorization'] = 'Token ' + token;
      //     }
      //     // config.headers['Content-Type'] = 'application/json';
      //     return config;
      //   },
      //   error => {
      //     Promise.reject(error)
      //   });
       
          axios.get("http://127.0.0.1:8000/events/list-event/",
        {
          'headers': {
            'Authorization' : 'Token ' + JSON.parse(localStorage.getItem('auth-token')).token,
            'Accept': 'application/json',
            'Content-Type': 'application/json;charset=UTF-8'
          }
        })
        .then((response => {
          
          console.log(response.data);
          
        }))
        .catch((error) => {
          console.log(error);
        });


    }



  }

};





</script>

<style>
</style>

