<style>
#div1 {
  /* background-image: url("./imageforpage/background.png"); */
  background-size: cover;
  background-color: #3d4c53;
}
/* #div2{
  position: absolute;
} */
</style>
<template>
  <div class="" id="div1">
    <div
      class="container is-widescreen"
      v-for="cart in cart"
      :key="cart.book_id"
      id="div2"
    >
      <br />
      <br />
      <div class="card ml-5 mr-5" style="background-color: #f9fcea">
        <div class="row">
          <div class="column is-3 ml-5">
            <figure class="image is-4by5 mt-3">
              <img :src="imagePath(cart.book_image)" />
            </figure>
            <div class="">
              <div class="column is-3"></div>
              <div class="column has-text-weight-semibold is-size-4 ml-1">
                {{ cart.book_title }}
              </div>
            </div>
          </div>
          <div class="column is-4 ml-2">
            <div class="field mt-4">
              <label class="label">STATUS</label>
              <div class="control">
                <input
                  disabled
                  class="input"
                  type="text"
                  placeholder="borrowed"
                /> 
              </div>
            </div>
            <div class="field mt-4">
              <label class="label">BORROW DATE</label>
              <div class="control">
                <input
                    v-if="cart.book_id == borrowCheck.book_id"
                 class="input"
                  v-model="borrowdate"
                  type="date"
                  placeholder="BORROW DATE"
                  />
                  <input
                  v-else-if="cart.book_id != borrowCheck.book_id"
                  disabled="disabled"
                  class="input"
                  type="date"
                  placeholder="2022-01-15"
                />
  
              </div>
            </div>
            <br />
            <div class="field mt-5">
              <label class="label">LIBRARIAN</label>

              <div class="control">
                 <input
                    v-if="cart.book_id == borrowCheck.book_id"
                 class="input" type="text" :value="user.librarian_id"
                  />
                  <input
                  v-else-if="cart.book_id != borrowCheck.book_id"
                  disabled="disabled"
                  class="input" type="text" :value="user.librarian_id"
                />
         
              </div>
            </div>
          </div>
          <div class="column is-4 ml-2">
            <div class="field mt-4">
              <label class="label">BORROW BY</label>
              <div class="control">
                 <input
                    v-if="cart.book_id == borrowCheck.book_id"
                 class="input"
                  v-model="borrowby"
                  type="text"
                  placeholder="BORROW BY"
                  />
                  <input
                  v-else-if="cart.book_id != borrowCheck.book_id"
                  disabled="disabled"
                  class="input"
                  v-model="borrowby"
                  type="text"
                  placeholder="BORROW BY"
                />

              </div>
            </div>
            <div class="field mt-4">
              <label class="label">DATELINE DATE</label>
              <div class="control">
                                 <input
                    v-if="cart.book_id == borrowCheck.book_id"
                  class="input"
                  v-model="datelinedate"
                  type="date"
                  placeholder="DATELINE DATE"
                  />
                  <input
                  v-else-if="cart.book_id != borrowCheck.book_id"
                  disabled="disabled"
                 class="input"
                  v-model="datelinedate"
                  type="date"
                  placeholder="DATELINE DATE"
                />
              </div>
            </div>
            <br />
            <br />
            <div class="row mt-4">
              <div class="column is-6">
                <button v-if="cart.book_id == borrowCheck.book_id" @click="confirm(cart, index)" class="button is-primary is-fullwidth">
                  CONFIRM
                </button>
                <button  v-else-if="cart.book_id != borrowCheck.book_id" @click="borrow(cart)" class="button is-primary is-fullwidth">
                  BORROW
                </button>
              </div>
              <div class="column">
                <button @click="cancel()" class="button is-danger is-fullwidth">
                  CANCEL
                </button>
              </div>
            </div>
            <br />
          </div>
        </div>
      </div>
      <br />
      <br />
      <br />
    </div>
  </div>
</template>

<script>
import axios from "@/plugins/axios";
// @ is an alias to /src
let text = localStorage.getItem("testJSON");
let obj = JSON.parse(text);
// console.log(obj);
export default {
  name: "Home",
  props: ["user"],

  data() {
    return {
      search: "",
      blogs: [],
      status: "",
      borrowdate: "",
      librarian: "",
      borrowby: "",
      datelinedate: "",
      bookid:"",
      cart:[],
      borrowCheck:""
      // text: obj
    };
  },
  created() {
    let text = localStorage.getItem("testJSON");
    let obj = JSON.parse(text);
    this.cart = obj;
  },
  mounted() {
    this.getBlogs();
  },
  methods: {
    getBlogs() {
      axios
        .get("http://localhost:3000", {
          params: {
            search: this.search,
          },
        })
        .then((response) => {
          this.blogs = response.data;
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
    confirm(data1, index) {
      const data = {
        status: this.status,
        borrowdate: this.borrowdate,
        librarian: this.user.librarian_id,
        borrowby: this.borrowby,
        datelinedate: this.datelinedate,
        bookid: data1.book_id
      };
       axios
          .post("http://localhost:3000/borrow", data)
          .then((res) => {
            if(this.cart.length > 1){
            this.cart = Array.from(this.cart);
            this.cart.splice(index, 1);
            this.status= "borrowed";
            this.borrowdate= "";
            this.librarian= "";
            this.borrowby= "";
            this.datelinedate= "";
            this.bookid=""
            console.log(this.cart.length)
          } else{
            this.status= "";
            this.borrowdate= "";
            this.librarian= "";
            this.borrowby= "";
            this.datelinedate= "";
            this.bookid=""
            this.$router.push("/home");
          }
          })
          .catch((err) => {
            console.log(err)
          });
      console.log(data);
      console.log(this.cart);
      // console.log(this.user)
      //  axios
      //   .post("http://localhost:3000/user/login/", data)
      //   .then((res) => {
      //     const token = res.data.token;
      //     localStorage.setItem("token", token);
      //     this.$emit("auth-change");
      //     this.$router.push({ path: "/home" });
      //   })
      //   .catch((error) => {
      //     this.error = error.response.data;
      //     console.log(error.response.data);
      //   });
    },
     cancel() {
      this.$router.push("/home");
    },
    borrow(data1){
       this.borrowCheck = this.cart.filter((val) => val.book_id === data1.book_id)[0];
    }
  },
};
</script>
