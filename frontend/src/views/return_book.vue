<style>
.bg {
  background-color: #3d4c53;
  padding-bottom: 4%;
  padding-top: 4%;
  background-size: 100% 100%;
}
#box {
  background-color: #f9fbed;
  height: 700px;
  overflow-x: hidden;
  overflow-y: scroll;
  scrollbar-width: none;
  border-radius: 10px;
}
#box::-webkit-scrollbar {
  display: none;
}
#nav-box {
  background-color: #e6772e;
  position: absolute;
  width: 97.8%;
  height: 60px;
  border-radius: 8px 8px 0 0;
  padding-left: 10px;
  padding-top: 10px;
  z-index: 1;
}
</style>
<template>
  <div class="bg">
    <div class="container is-widescreen">
      <div
        v-for="(data, index) in returndata1"
        :key="data.book_id"
        class="page my-5"
        id="box"
      >
        <div class="in is-centered" id="nav-box">
          <label class="labeld">
            <p class="title is-3">
              Member Name : {{ data.p_firstname }} {{ data.p_lastname }}
            </p>
          </label>
        </div>
        <br />
        <br />
        <div class="box mb-5 mt-5" style="background-color: #e1ebea">
          <div class="columns is-centered">
            <div class="column is-4" style="text-align: center">
              <img :src="imagePath(data.book_image)" alt="Placeholder image" />
              <label class="label">{{ data.book_title }}</label>
            </div>
            <div class="column is-4">
              <fieldset disabled class="mb-5">
                <label class="label" style="z-index: -1">BORROW DATE</label>
                <div class="control">
                  <!-- <input class="input" type="text" placeholder="01-01-2021" /> -->
                  <p>{{ data.borrow_date }}</p>
                </div>
              </fieldset>

              <fieldset disabled>
                <label class="label" style="z-index: -1"
                  >RIBRARIAN</label
                >
                <div class="control">
                  <!-- <input class="input" type="text" placeholder="TEE" /> -->
                  <p>{{ data.br_librarian_id }}</p>
                </div>
              </fieldset>
            </div>
            <div class="column is-4">
              <fieldset>
                <label class="label">DATELINE DATE</label>
                <div class="control">
                  <input
                    v-if="data.book_id == returnCheck.book_id"
                    v-model="return_date"
                    class="input"
                    type="date"
                    placeholder="2022-01-15"
                  />
                  <input
                  v-else-if="data.book_id != returnCheck.book_id"
                  disabled="disabled"
                  class="input"
                  type="date"
                  placeholder="2022-01-15"
                />
                  <p
                  
                  class="help is-danger"
                  v-if="return_date > data.borrow_dateline_date && data.book_id == returnCheck.book_id"
                >
                  return to late
                </p>
                </div>
              </fieldset>

              <div class="columns is-centered mb-2" style="padding-top: 50%">
                <button v-if="data.book_id == returnCheck.book_id" @click="confirm(data, index)" class="button is-primary">
                  <span class="icon is-small">
                    <svg
                      xmlns="http://www.w3.org/2000/svg"
                      width="16"
                      height="16"
                      fill="currentColor"
                      class="bi bi-caret-left"
                      viewBox="0 0 16 16"
                    >
                      <path
                        d="M10 12.796V3.204L4.519 8 10 12.796zm-.659.753-5.48-4.796a1 1 0 0 1 0-1.506lq5.48-4.796A1 1 0 0 1 11 3.204v9.592a1 1 0 0 1-1.659.753z"
                      />
                    </svg>
                  </span>
                  <span>CONFIRM</span>
                </button>
                <button v-else-if="data.book_id != returnCheck.book_id" @click="summit(data)" class="button is-danger">
                  <span class="icon is-small">
                    <svg
                      xmlns="http://www.w3.org/2000/svg"
                      width="16"
                      height="16"
                      fill="currentColor"
                      class="bi bi-caret-left"
                      viewBox="0 0 16 16"
                    >
                      <path
                        d="M10 12.796V3.204L4.519 8 10 12.796zm-.659.753-5.48-4.796a1 1 0 0 1 0-1.506lq5.48-4.796A1 1 0 0 1 11 3.204v9.592a1 1 0 0 1-1.659.753z"
                      />
                    </svg>
                  </span>
                  <span>RETURN</span>
                </button>
                <button @click="cancal()" class="button is-danger mx-5">
                  <span>CANCAL</span>
                </button>
              </div>
            </div>
          </div>
        </div>
      </div>
      <div class="column is-1"></div>
    </div>
  </div>
</template>
<script>
import axios from "@/plugins/axios";

export default {
  name: "Home",
  props: ["user"],
  data() {
    return {
      cart: [],
      returndata: [],
      returndata1: [],
      return_date: "",
      returnCheck:"",
    };
  },
  created() {
    let text = localStorage.getItem("testJSON");
    let obj = JSON.parse(text);
    this.cart = obj;
    this.getreturn();
  },
  methods: {
    getreturn() {
      axios
        .get("http://localhost:3000/borrow_return")
        .then((response) => {
          this.returndata = response.data.returndata.filter(
            (val) => val.book_id == this.cart.book_id && val.return_date == null
          );
          this.returndata1 = response.data.returndata.filter(
            (val) =>
              val.br_member_id == this.returndata[0].member_id &&
              val.return_date == null
          );
          console.log(this.returndata1);
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
    confirm(data1,index) {
      const data = {
        bookid: data1.book_id,
        return_date: this.return_date,
      };
      axios
        .post("http://localhost:3000/return", data)
        .then((res) => {
          if(this.returndata1.length > 1){
            this.returndata1 = Array.from(this.returndata1);
            this.returndata1.splice(index, 1);
            this.return_date = ""
          } else{
            this.return_date = ""
            this.$router.push("/home");
          }
          // this.$router.push("/home");
        })
        .catch((err) => {
          alert(err.response.data.details.message);
        });
    },
    summit(data1){
      this.returnCheck = this.returndata1.filter((val) => val.book_id === data1.book_id)[0];
    },
    cancal() {
      this.$router.push("/home");
    },
  },
};
</script>

