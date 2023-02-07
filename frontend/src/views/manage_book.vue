<style>
.bg {
  background-image: url("./imageforpage/background.png");
  padding-bottom: 15%;
  padding-top: 9%;
  background-size: 100% 100%;
}
</style>
<template>
  <div class="bg">
    <div class="container is-widescreen">
      <div class="box mt-5 has-background-warning-light">
        <div class="columns is-centered">
          <label class="labeld">
            <p class="title is-3">MANAGE BOOK</p>
          </label>
        </div>

        <div class="field mb-1">
          <div class="columns is-variable is-1">
            <div class="column">
              <label class="label">Title</label>
              <div class="control">
                <input v-model="title" class="input" type="text" placeholder="Title" />
              </div>
            </div>

            <div class="column">
              <label class="label">Language</label>
              <div class="control">
                <input v-model="language" class="input" type="text" placeholder="Language" />
              </div>
            </div>

            <div class="column">
              <label class="label">Type</label>
              <div class="control">
                <input v-model="type" class="input" type="text" placeholder="Type" />
              </div>
            </div>

            <div class="column">
              <label class="label">isbn</label>
              <div class="control">
                <input v-model="isbn" class="input" type="text" placeholder="isbn" />
              </div>
            </div>
          </div>
        </div>

        <div class="field mb-1">
          <div class="columns is-variable is-1">
            <div class="column">
              <label class="label">Book_year</label>
              <div class="control">
                <input v-model="bookyear" class="input" type="text" placeholder="Book year" />
              </div>
            </div>

            <div class="column">
              <label class="label">Edition</label>
              <div class="control">
                <input v-model="edition" class="input" type="text" placeholder="Edition" />
              </div>
            </div>

            <div class="column">
              <label class="label">Accesnumber</label>
              <div class="control">
                <input v-model="accesnumber" class="input" type="text" placeholder="Accesnumber" />
              </div>
            </div>

            <div class="column">
              <label class="label">Publisher</label>
              <div class="control">
                <input v-model="publisher" class="input" type="text" placeholder="Publisher" />
              </div>
            </div>
          </div>
        </div>

        <div class="field mb-1">
          <div class="columns is-variable is-1">
            <div class="column">
              <label class="label">Book_quantity</label>
              <div class="control">
                <input v-model="quantity" class="input" type="text" placeholder="Book quantity" />
              </div>
            </div>

            <div class="column">
              <label class="label">Price</label>
              <div class="control">
                <input v-model="price" class="input" type="text" placeholder="Price" />
              </div>
            </div>

            <div class="column">
              <label class="label">Status</label>
              <div class="control">
                <input v-model="status" class="input" type="text" placeholder="Status" />
              </div>
            </div>

            <div class="column">
              <label class="label">Barcode</label>
              <div class="control">
                <input v-model="barcode" class="input" type="text" placeholder="Barcode" />
              </div>
            </div>
          </div>
        </div>

        <div class="field mb-1 mb-5">
          <div class="columns is-variable is-1">
            <div class="column">
              <label class="label">Image</label>
              <label class="file-label">
                <input
                  class="mb-5"
                  multiple
                  type="file"
                  accept="image/png, image/jpeg, image/webp"
                  @change="selectImages"
                />
              </label>
            </div>

            <div class="column">
              <label class="label">Create By</label>
              <div class="control">
                <input v-model="createby" class="input" type="text" placeholder="Lastname" />
              </div>
            </div>
          </div>
        </div>

        <div class="columns is-centered mb-2">
          <button @click="submitBlog()" class="button is-primary">
            <span class="icon is-small">
              <i class="fa fa-check"></i>
            </span>
            <span>SAVE</span>
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  data() {
    return {
      images: [], // array of image
      title: "",
      language: "",
      type: "",
      isbn: "",
      bookyear: "",
      edition: "",
      accesnumber: "",
      publisher:"",
      quantity:"",
      price:"",
      status:"",
      barcode:"",
      createby:"",
    };
  },
  methods: {
    selectImages(event) {
      this.images = event.target.files;
    },
    showSelectImage(image) {
      // for preview only
      return URL.createObjectURL(image);
    },
    deleteSelectImage(index) {
      console.log(this.images);
      this.images = Array.from(this.images);
      this.images.splice(index, 1);
    },
    submitBlog() {
      let formData = new FormData();
      formData.append("title", this.title);
      formData.append("language", this.language);
      formData.append("type", this.type);
      formData.append("isbn", this.isbn);
      formData.append("bookyear", this.bookyear);
      formData.append("edition", this.edition);
      formData.append("accesnumber", this.accesnumber);
      formData.append("publisher", this.publisher);
      formData.append("quantity", this.quantity);
      formData.append("price", this.price);
      formData.append("status", this.status);
      formData.append("barcode", this.barcode);
      formData.append("createby", this.createby);
      this.images.forEach((image) => {
        formData.append("myImage", image);
      });
      console.log(formData);

    //   // Note ***************
    //   // ตอนเรายิง Postmant จะใช้ fromData
    //   // ตอนยิงหลาย ๆ รูปพร้อมกันใน Postman จะเป็นแบบนี้

    //   // title   | "This is a title of blog"
    //   // comment | "comment in blog"
    //   // ...
    //   // myImage | [select file 1]
    //   // myImage | [select file 2]
    //   // myImage | [select file 3]

    //   // จะสังเกตุว่าใช้ myImage เป็น key เดียวกัน เลยต้องเอามา loop forEach
    //   // พอไปฝั่ง backend มันจะจัด file ให้เป็น Array เพื่อเอาไปใช้งานต่อได้

      axios
        .post("http://localhost:3000/addBook", formData)
        .then((res) => this.$router.push("/home"))
        .catch((e) => console.log(e.response.data));
    },
  },
};
</script>

<style>
</style>