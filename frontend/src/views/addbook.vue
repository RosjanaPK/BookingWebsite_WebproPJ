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
          ตี๋ทำอยู่ว
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
export default {
  name: "Home",
  props: ["user"],
  data() {
    return {
      search: "",
      blogs: [],
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
  },
};
</script>
