<style>
@import url("https://fonts.googleapis.com/css2?family=Bebas+Neue&display=swap");
body {
  /* background-image: linear-gradient(
      rgba(220, 130, 26, 0.5),
      rgba(60, 55, 157, 0.5)
    ),
    url("./imageforpage/bgtest.png");
  background-size: cover;
  min-height: 100%;
  width: 100%;
  position: fixed; */
  font-family: "Bebas Neue", cursive;
}
#div1 {
  background-image: url("./imageforpage/background.png");
  background-size: cover;
}
/* #div2{
  position: absolute;
} */
</style>
<template>
  <div class="" id="">
    <div class="container-fluid has-background-grey-lighter">
      <p class="text-center" style="font-size: 30px">MY PROFILE</p>
      <div class="row">
        <div class="column is-6">
          <article class="message is-dark mt-3">
            <div class="message-body">
              <div class="row" v-if="check == 0" style="font-size: 20px">
                <p class="column is-6">
                  Name: {{ user.p_firstname }} {{ user.p_lastname }}
                </p>
                <p class="column is-6">Email: {{ user.p_email }}</p>
                <p class="column is-6">Mobile: {{ user.p_mobile }}</p>
                <p class="column is-6">Password: {{ user.p_password }}</p>
                <p class="column is-12">Address: {{ user.p_address }}</p>
              </div>
              <div
                v-else-if="check == 1"
                class="row container"
                style="font-size: 20px"
              >
                <label class="">
                  FIRSTNAME
                  <input
                    v-model="firstname"
                    class="input column is-12"
                    type="text"
                /></label>
                <label class="ml-5">
                  LASTNAME
                  <input
                    v-model="lastname"
                    class="input column is-12"
                    type="text"
                /></label>
                <label class="">
                  EMAIL<input
                    v-model="email"
                    class="input column is-12"
                    type="text"
                /></label>
                <label class="ml-5">
                  PASSWORD<input
                    v-model="password"
                    class="input column is-12"
                    type="text"
                /></label>
                <label class="">
                  MOBILE<input
                    v-model="mobile"
                    class="input column is-12"
                    type="text"
                /></label>
                <label class="row ml-1">
                  ADDRESS
                  <textarea
                    v-model="address"
                    class="textarea"
                    placeholder="Address"
                  ></textarea>
                </label>
              </div>
            </div>
          </article>
          <div>
            <button v-if="check == 0" @click="edit()" class="button is-danger">
              EDIT
            </button>
            <button
              v-else-if="check == 1"
              @click="submit()"
              class="button is-success"
            >
              SUBMIT
            </button>
          </div>
        </div>
        <div class="column is-6">
          <div class="column" v-for="image in checkbook" :key="image.borrow_id">
            <article class="message is-info">
              <div class="message-header">
                <p class="has-text-black">{{ image.book_title }}</p>
              </div>
              <div class="message-body">
                <div class="row">
                  <div class="column is-4">
                    <div class="card">
                      <div class="card-image">
                        <figure class="image">
                          <img
                            style="height: 200px"
                            :src="imagePath(image.book_image)"
                            alt="Placeholder image"
                          />
                        </figure>
                      </div>
                    </div>
                  </div>
                  <div class="column is-6">
                    <p style="font-size: 20px">
                      <strong>TITLE : </strong> {{ image.book_title }}
                    </p>
                    <p style="font-size: 20px">
                      <strong>BORROW DATE : </strong>
                      {{ image.borrow_date.slice(0, 10) }}
                    </p>
                    <p style="font-size: 20px">
                      <strong>DATELINE DATE : </strong>
                      {{ image.borrow_dateline_date.slice(0, 10) }}
                    </p>
                  </div>
                </div>
              </div>
            </article>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "@/plugins/axios";
// @ is an alias to /src
export default {
  name: "Home",
  // props: ["user"],
  data() {
    return {
      search: "",
      book: [],
      user: "",
      checkbook: [],
      check: 0,
      firstname: "",
      lastname: "",
      password: "",
      email: "",
      address: "",
      mobile: "",
      id: "",
      checklibrarian: 0,
    };
  },
  mounted() {
    this.getprofile();
    this.getBlogs();
  },
  methods: {
    getprofile() {
      axios
        .get("http://localhost:3000/blogs")
        .then((response) => {
          this.checkbook = response.data.images;
          console.log(this.checkbook);
        })
        .catch((err) => {
          console.log(err);
        });
    },
    getBlogs() {
      axios
        .get("http://localhost:3000/", {
          params: {
            search: this.search,
          },
        })
        .then((response) => {
          this.book = response.data.rows;
          this.user = response.data.user;

          console.log("nnn", this.book);
          console.log("User", this.user);
        })
        .catch((err) => {
          console.log(err);
        });
    },
    imagePath(file_path) {
      if (file_path) {
        return "http://localhost:3000/" + file_path;
      } else {
        return "https://bulma.io/images/placeholders/640x360.png";
      }
    },
    edit() {
      this.check = 1;
      this.firstname = this.user.p_firstname;
      this.lastname = this.user.p_lastname;
      this.email = this.user.p_email;
      this.password = this.user.p_password;
      this.address = this.user.p_address;
      this.mobile = this.user.p_mobile;
    },
    submit() {
      this.check = 0;
      if (this.user.member_id == null) {
        this.id = this.user.librarian_id;
        this.checklibrarian = 1;
      } else {
        this.id = this.user.member_id;
        this.checklibrarian = 0;
      }
      const data = {
        firstname: this.firstname,
        lastname: this.lastname,
        email: this.email,
        password: this.password,
        address: this.address,
        mobile: this.mobile,
        id: this.id,
        check: this.checklibrarian,
      };
      console.log(data);
      axios
        .post("http://localhost:3000/user/update", data)
        .then((res) => {
          this.user = res.data.rows[0];
        })
        .catch((err) => {
          alert(err.response.data.details.message);
        });
    },
  },
};
</script>
