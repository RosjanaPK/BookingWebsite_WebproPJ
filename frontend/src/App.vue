<template>
  <div id="app">
    <!-- nav bar -->
    <div v-if="!user"><router-view :key="$route.fullPath" @auth-change="onAuthChange" :user="user" /></div>
    <div v-if="user">
    <nav class="navbar" role="navigation" aria-label="main navigation">
      <div class="navbar-brand">
        <router-link to="/home" class="navbar-item is-size-4" style="color:#596968">
          Librarian Online
        </router-link>
        <a
          role="button"
          class="navbar-burger"
          aria-label="menu"
          aria-expanded="false"
          data-target="navbarBasicExample"
        >
          <span aria-hidden="true"></span>
          <span aria-hidden="true"></span>
          <span aria-hidden="true"></span>
        </a>
      </div>

      <div class="navbar-menu">
        <div class="navbar-end">
          <div v-if="user_id  && user_id != undefined" class="navbar-item">
            <router-link to="/user/signuplibrarian" class="button is-primary">
              <strong>Sign up Librarian</strong>
            </router-link>
          </div>
          <div v-if="user" class="navbar-item has-dropdown is-hoverable">
            <a class="navbar-link">
              <figure class="image is-24x24 my-auto">
                <img
                  class="is-rounded"
                  src="https://bulma.io/images/placeholders/128x128.png"
                />
              </figure>
              <span class="pl-3"
                >{{ user.p_firstname }} {{ user.p_lastname }}</span
              >
            </a>
            <div class="navbar-dropdown">
              <a class="navbar-item" @click="profilepage()">Profile</a>
              <a class="navbar-item" @click="DeletePassword()">Log out</a>
            </div>
          </div>
          <div v-if="!user" class="navbar-item">
            <router-link to="/">
              <strong>Main</strong>
            </router-link>
          </div>
          <div v-if="!user" class="navbar-item">
            <router-link to="/user/login">
              <strong>Login</strong>
            </router-link>
          </div>
          <div v-if="!user" class="navbar-item">
            <router-link to="/user/signup">
              <strong>Signup</strong>
            </router-link>
          </div>
        </div>
      </div>
    </nav>
    
    <router-view :key="$route.fullPath" @auth-change="onAuthChange" :user="user" />
  </div></div>
</template>

<script>
import axios from '@/plugins/axios'

export default {  
  data () {    
    return {      
      user: null,
      user_id: "",
      }  
  },
  mounted () {
    this.onAuthChange()
  },
  methods: {
    onAuthChange () {
      const token = localStorage.getItem('token')
      if (token) {
        this.getUser()
      }
    },
    getUser () {
      const token = localStorage.getItem('token')
      axios.get('/user/me').then(res => {
        this.user = res.data
        this.user_id = this.user.librarian_id
      })
    },
    DeletePassword() {
      axios
        .delete(`http://localhost:3000/user/logout`)
        .then((response) => {
          localStorage.removeItem("token")
          this.user = null
          this.user_id = ""
          this.$router.push("/user/login");
        })
        .catch((error) => {
          alert(error.response.data.message);
        });
    },
    profilepage(){
      this.$router.push("/user/profile");
    }
  }}
</script>