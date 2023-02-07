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
    <div class="container is-widescreen" id="div2">
      <br />
      <br />
      <div class="card ml-5 mr-5" style="background-color: #f9fcea">
        <div class="row">
          <div class="column is-3 ml-5">
            <figure class="image is-3by4 mt-3 ml-5">
              <img :src="imagePath(cart.book_image)" />
            </figure>
          </div>
          <div class="column is-2"></div>
          <div class="colimn is-7">
            <div class="row field mt-4">
              <label class="column is-5 label mt-2">BOOK TITLE</label>
              <div class="column control">
                <input
                  class="input"
                  type="text"
                  v-model="booktitle"
                  placeholder="BOOK TITLE"
                />
              </div>
            </div>
            <div class="row field">
              <label class="column is-5 label mt-2">BOOK TYPE</label>
              <div class="column control">
                <input
                  class="input"
                  type="text"
                  v-model="booktype"
                  placeholder="BOOK TYPE"
                />
              </div>
            </div>
            <div class="row field">
              <label class="column is-5 label mt-2">BOOK YEAR</label>
              <div class="column control">
                <input
                  class="input"
                  type="text"
                  v-model="bookyear"
                  placeholder="BOOK YEAR"
                />
              </div>
            </div>
            <div class="row field">
              <label class="column is-5 label mt-2">BOOK LANGUAGE</label>
              <div class="column control">
                <input
                  class="input"
                  type="text"
                  v-model="booklanguage"
                  placeholder="BOOK LANGUAGE"
                />
              </div>
            </div>
            <div class="row field">
              <label class="column is-5 label mt-2">BOOK PUBLISHER</label>
              <div class="column control">
                <input
                  class="input"
                  type="text"
                  v-model="bookpublisher"
                  placeholder="BOOK PUBLISHER"
                />
              </div>
            </div>
            <div class="row field">
              <label class="column is-5 label mt-2">BOOK PRICE</label>
              <div class="column is-7 control">
                <input
                  class="input"
                  type="text"
                  v-model="bookprice"
                  placeholder="BOOK PRICE"
                />
              </div>
            </div>
            <div class="row">
              <div class="column is-5 ml-5"></div>
              <div class="column is-5 ml-5">
                <button
                  @click="deletebook()"
                  class="button is-danger is-fullwidth ml-4"
                >
                  DELETE
                </button>
              </div>
            </div>
          </div>
        </div>
      </div>
      <div class="row">
        <div class="column is-8"></div>
        <div class="column is-2">
          <button @click="save()" class="button is-success is-fullwidth ml-4">
            SAVE
          </button>
        </div>
        <div class="column is-2">
          <button @click="cancel()" class="button is-danger is-fullwidth ml-4">
            CANCEL
          </button>
        </div>
      </div>
      <br />
    </div>
  </div>
</template>

<script>
import axios from "@/plugins/axios";
// @ is an alias to /src
export default {
  name: "Home",
  props: ["user"],
  data() {
    return {
      search: "",
      blogs: [],
      cart: [],
      booktitle: "",
      booktype: "",
      bookyear: "",
      booklanguage: "",
      bookpublisher: "",
      bookprice: "",
      bookid: "",
    };
  },
  created() {
    let text = localStorage.getItem("testJSON");
    let obj = JSON.parse(text);
    this.cart = obj;
    this.booktitle = this.cart.book_title;
    this.booktype = this.cart.book_type;
    this.bookyear = this.cart.book_year;
    this.booklanguage = this.cart.book_language;
    this.bookpublisher = this.cart.book_publisher;
    this.bookprice = this.cart.book_price;
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
    shortContent(content) {
      if (content.length > 200) {
        return content.substring(0, 197) + "...";
      }
      return content;
    },
    addLike(blogId) {
      axios
        .put(`http://localhost:3000/blogs/addlike/${blogId}`)
        .then((response) => {
          let selectedBlog = this.blogs.filter((e) => e.id === blogId)[0];
          selectedBlog.like = response.data.like;
        })
        .catch((err) => {
          console.log(err);
        });
    },
    isBlogOwner(blog) {
      if (!this.user) return false;
      return blog.create_by_id === this.user.id || this.user.role === "admin";
    },
    cancel() {
      this.$router.push("/home");
    },
    save() {
      const data = {
        booktitle: this.booktitle,
        booktype: this.booktype,
        bookyear: this.bookyear,
        booklanguage: this.booklanguage,
        bookpublisher: this.bookpublisher,
        bookprice: this.bookprice,
        bookid: this.cart.book_id,
      };
      axios
        .post("http://localhost:3000/edit", data)
        .then((res) => {
          this.$router.push("/home");
        })
        .catch((err) => {
          alert(err.response.data.details.message);
        });
    },
    deletebook() {
        
      const data = {
        bookid: this.cart.book_id,
      };
      console.log(data)
      axios
        .delete(`http://localhost:3000/delete/${this.cart.book_id}`)
        .then((res) => {
          this.$router.push("/home");
        })
        .catch((err) => {
          alert(err.response.data.details.message);
        });
    },
  },
};
</script>
