<style>
@import url("https://fonts.googleapis.com/css2?family=Bebas+Neue&display=swap");
body{
  position: none;
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
  <div class="columns" id="div1">
    <div   v-if="user.librarian_id && user.information_service_dept == 1 && checkshow == 0" class="column"></div>
    <div   v-if="user.member_id || user.information_service_dept == 0 || checkshow == 1" class="column is-2"></div>

    <!-- Column แสดงสินค้า--------------------------------------------------------->
    <div class="column is-8 pt-6">
    
      <div class="">
        <div class="columns">
          <div class="column is-two-thirds">
          
            <input
              class="input"
              type="text"
              v-model="search"
              placeholder="Search book"
            />
          </div>
          <div class="column">
            <button @click="getBlogs" class="button">Search</button>
          </div>
          <div class="column">
            <button
              v-if="user.librarian_id && user.cataloging_dept == 1"
              @click="addBook()"
              class="button is-primary"
            >
              Add Book
            </button>
          </div>
          <div class="column">
            <button
              v-if="
                user.librarian_id &&
                check == false &&
                user.information_service_dept == 1
              "
              @click="retrunBook()"
              class="button is-primary"
            >
              Retrun Book
            </button>
            <button
              v-if="user.librarian_id && check == true"
              @click="homeBook()"
              class="button is-primary"
            >
              Home
            </button>
          </div>
        </div>
      </div>
      <h1 class="is-size-4 mb-4 has-text-white mt-5">All Products ({{ book.length }})</h1>
      
      <div class="container is-max-desktop">
        <div class="is-multiline columns is-variable is-2">
          <!-- Card element start here------------------------------------------>
          <div
            id="card_product"
            class="column is-one-quarter"
            v-for="book1 in book"
            :key="book1.book_id"
          >
            <div class="card">
              <div class="card-image pt-5">
                <figure class="image">
                  <img
                    style="height: 300px"
                    :src="imagePath(book1.book_image)"
                    alt="Placeholder image"
                  />
                </figure>
              </div>
              <div class="card-content">
                <div class="title" style="font-size: 20px">
                  {{ book1.book_title }}
                </div>
                <div class="content" style="height: 50px">
                  <!-- {{ shortContent(blog.content) }} -->
                  Type : {{ book1.book_type }}
                </div>
                <div class="row">
                  <div class="column is-1"></div>
                  <button
                    type="button"
                    class="btn btn-danger has-text-weight-semibold mr-1"
                    v-if="user.librarian_id"
                    style="font-size: 14px"
                    @click="edit(book1)"
                  >
                    edit
                  </button>
                  <button
                    type="button"
                    class="btn btn-warning has-text-weight-semibold"
                    v-if="
                      user.librarian_id &&
                      book1.book_status == 'unborrow' &&
                      user.information_service_dept == 1
                    "
                    style="font-size: 14px"
                    @click="addToCart(book1)"
                  >
                    borrow
                  </button>
                  <button
                    type="button"
                    class="btn btn-info has-text-weight-semibold"
                    v-else-if="
                      user.librarian_id &&
                      book1.book_status == 'borrowed' &&
                      user.information_service_dept == 1
                    "
                    style="font-size: 16px"
                    @click="returnbook(book1)"
                  >
                    Return
                  </button>
                </div>
              </div>
              <!-- <div class="card-footer" style="background-color: #f2c249">
                  <router-link
                    class="card-footer-item"
                    :to="`/blogs/detail/${blog.id}`"
                    >Borrow</router-link
                  >
                </div> -->
            </div>
          </div>
        </div>
      </div>
    </div>

    <!-- Column แสดงตะกร้า--------------------------------------------------->
    <div  v-if="user.librarian_id && user.information_service_dept == 1 && checkshow == 0" class="column is-3 pt-6 pl-0 pr-5">
      <div  style="display: flex; justify-content: space-between">
        <span class="is-size-4 mb-4 has-text-white">LIST</span>
        <a @click="clear()" class="is-danger mb-4 button has-text-white">Clear</a>
      </div>

      <!-- Card element start here ------------------------------------------>
      <div  v-for="product in cart" :key="product.book_id" class="card mb-4">
        <div class="card-content p-0">
          <div class="media">
            <div class="media-left">
              <figure class="image">
                <img
                  style="height: 100px"
                  :src="imagePath(product.book_image)"
                  alt="Placeholder image"
                />
              </figure>
            </div>
            <div class="media-content pt-2">
              <p class="is-5">{{ product.book_title }}</p>
              <p class="has-text-grey-light is-6">{{ product.book_type }}</p>
              <div style="display: flex; justify-content: space-between">
                <div>
                  <!-- ราคาสินค้า------------------------------------------------ -->
                  <span class="is-6 has-text-danger ">{{
                    product.book_price
                  }}</span>
                  <!-- จำนวนสินค้า----------------------------------------------- -->
                  <span >x 1 </span>
                </div>
                <div>
                  <!-- icon รูปถังขยะ------------------------------------------- -->
                  <span
                    class="icon mr-2"
                    key="false"
                    @click="removeFromCart(product)"
                  >
                    <i class="far fa-trash-alt"></i>
                  </span>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
      <div
        style="
          display: flex;
          justify-content: space-between;
          font-size: 1.25rem;
        "
      >
        <span class="has-text-weight-bold has-text-white">Total</span>
        <span class="has-text-white" id="totalPrice">{{ cart.length }}</span>
      </div>

      <!-- ปุ่ม Checkout ------------------------------------------------------------ -->
      <a
        class="button is-warning mt-3"
        style="width: 100%"
        @click="borrow(cart)"
        >BORROW</a
      >
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
      borrowed: [],
      retrun: [],
      check: false,
      cart: [],
      checklist: [],
      checkshow: 0,
      topbook: [],
    };
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
          this.user = response.data.user;
          if (this.user.member_id == undefined) {
            this.book = response.data.rows;
            this.book = this.book.filter(
              (val) => val.book_status == "unborrow"
            );
          } else {
            this.book = response.data.rows;
          }
          // this.retrun = this.book.filter((val) => val.book_status == 'unborrow')
        })
        .catch((err) => {
          console.log(err);
        });
    },
    imagePath(book_image) {
      if (book_image) {
        return "http://localhost:3000/" + book_image;
      } else {
        return "https://bulma.io/images/placeholders/640x360.png";
      }
    },
    addBook() {
      this.$router.push("/user/manage_book");
    },
    borrow(data) {
      // this.stock= data
      if(this.cart.length > 0){
        const myJSON = JSON.stringify(data);
        localStorage.setItem("testJSON", myJSON);
        this.$router.push("/user/borrow");
      }else{
        this.$router.push("/home");
      }

    },
    edit(data) {
      console.log(data);
      const myJSON = JSON.stringify(data);
      localStorage.setItem("testJSON", myJSON);
      this.$router.push("/user/edit");
    },
    returnbook(data) {
      // this.stock= data
      const myJSON = JSON.stringify(data);
      localStorage.setItem("testJSON", myJSON);
      this.$router.push("/user/return");
    },
    retrunBook() {
      this.check = !this.check;
      this.checkshow = 1
      axios
        .get("http://localhost:3000", {
          params: {
            search: this.search,
          },
        })
        .then((response) => {
          this.book = response.data.rows;
          this.user = response.data.user;
          this.book = this.book.filter((val) => val.book_status == "borrowed");
        })
        .catch((err) => {
          console.log(err);
        });
    },
    homeBook() {
      this.check = !this.check;
      this.checkshow = 0
      this.getBlogs();
    },
    addToCart(product) {
      this.cart.push(product);
      product.book_quantity = 1;
      for (let i = 0; i < this.book.length; i++) {
        if (this.book[i].book_id === product.book_id) {
            this.book.splice(i, 1);
            this.checklist.push(product)
        }
      }
  
    },
    removeFromCart(product2) {
      let num = 0
      for (let i = 0; i < this.cart.length; i++) {
        if (this.cart[i].book_title === product2.book_title) {          
          if (this.cart[i].book_quantity > 1) {
            this.cart[i].book_quantity -= 1;
          } else {
            for(let j = 0; j < this.book.length; j++){
              // if (this.cart[i].book_id == this.book[j].book_id ) {
                // console.log(this.book[j].book_id)
                // console.log('iiiii', this.cart[i].book_id)
                // console.log(this.checknum)
              // }
              // else{
              //   console.log('not ff')
              // }
            }
            this.cart.splice(i, 1);
            
          }
        } 
       
        
      }
      //  return this.book
      // console.log(num)
      this.book.push(product2)
      console.log(this.book.length)
    },
   
    clear(){
      this.cart = []
      console.log(this.checklist)
      for(let i = 0; i < this.checklist.length; i++){
        this.book.push(this.checklist[i])
      }
      this.checklist = []
    }
  },
};
</script>
