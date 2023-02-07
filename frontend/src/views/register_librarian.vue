<style>
.bg {
  background-image: url("./imageforpage/background.png");
  /* background-size: cover; */
  padding-bottom: 10%;
  padding-top: 7%;
  /* background-repeat: no-repeat; */
  background-size: 100% 100%;
}
</style>
<template>
  <div class="bg">
    <div class="container is-widescreen">
      <div class="box mt-5 has-background-warning-light">
        <div class="columns is-centered">
          <label class="labeld">
            <p class="title is-3">REGISTER FOR LIBRARIAN</p>
          </label>
        </div>

        <div class="field mb-1">
          <div class="columns is-variable is-1">
            <div class="column">
              <label class="label">Username</label>
              <div class="control">
                <input v-model="$v.username.$model" class="input" type="text" placeholder="Username" />
              </div>
            </div>

            <div class="column">
              <label class="label">Password</label>
              <div class="control">
                <input v-model="$v.password.$model" class="input" type="text" placeholder="Password" />
              </div>
            </div>
            <div class="column">
              <label class="label">Confirm Password</label>
              <div class="control">
                <input
                  v-model="$v.confirm_password.$model"
                  class="input"
                  type="text"
                  placeholder="Confirm Password"
                />
              </div>
            </div>
          </div>
        </div>

        <div class="field mb-1">
          <div class="columns is-variable is-1">
            <div class="column">
              <label class="label">First name</label>
              <div class="control">
                <input v-model="$v.first_name.$model" class="input" type="text" placeholder="First name" />
              </div>
            </div>
            <div class="column">
              <label class="label">Last name</label>
              <div class="control">
                <input v-model="$v.last_name.$model" class="input" type="text" placeholder="Last name" />
              </div>
            </div>
            <div class="column">
              <label class="label">email</label>
              <div class="control">
                <input v-model="$v.email.$model" class="input" type="text" placeholder="email" />
              </div>
            </div>
          </div>
        </div>

        <div class="field">
          <div class="columns is-variable is-1">
            <div class="column">
              <label class="label">Job Position</label>
              <div class="control is-expanded mb-5">
                <label class="radio is-size-6">
                  <input
                    v-model="$v.JobPosition.$model"
                    type="radio"
                    name="answer"
                    value="c1"
                  />
                  cataloging
                </label>
                <label class="radio is-size-6">
                  <input
                    v-model="$v.JobPosition.$model"
                    type="radio"
                    name="answer"
                    value="h1"
                  />
                  information_service
                </label>
              </div>

              <label class="label">Mobile</label>
              <p class="control is-expanded">
                <input v-model="$v.mobile.$model" class="input" type="text" placeholder="Mobile" />
              </p>
            </div>
            <div class="column">
              <div class="field">
                <label class="label">Address</label>
                <div class="control">
                  <textarea v-model="$v.address.$model" class="textarea" placeholder="Address"></textarea>
                </div>
              </div>
            </div>
          </div>
        </div>

        <div class="field mb-5">
          <div class="columns is-variable is-1">
            <div class="column">
              <label class="label">Faculty</label>
              <div class="control">
                <input v-model="$v.Faculty.$model" class="input" type="text" placeholder="Faculty" />
              </div>
            </div>
            <div class="column">
              <label class="label">Department</label>
              <div class="control">
                <input v-model="$v.Deparment.$model" class="input" type="text" placeholder="Department" />
              </div>
            </div>

            <div class="column">
              <label class="label">Function</label>
              <div class="control">
                <input v-model="$v.Function.$model" class="input" type="text" placeholder="Function" />
              </div>
            </div>
          </div>
        </div>

        <div class="columns is-centered mb-2">
          <div class="control is-centered">
            <button @click="submit()" class="button is-primary">
              <span>SING UP</span>
              <span class="icon is-small">
                <i class="fa fa-angle-right"></i>
              </span>
            </button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

// <script>
import axios from "@/plugins/axios";
import {
  required,
  email,
  helpers,
  minLength,
  maxLength,
  sameAs,
} from "vuelidate/lib/validators";

function mobile(value) {
  return !helpers.req(value) || !!value.match(/0[0-9]{9}/);
}

function complexPassword(value) {
  if (!(value.match(/[a-z]/) && value.match(/[A-Z]/) && value.match(/[0-9]/))) {
    return false;
  }
  return true;
}

export default {
  data() {
    return {
      username: "",
      password: "",
      confirm_password: "",
      first_name: "",
      last_name: "",
      email: "",
      JobPosition: "",
      address: "",
      mobile: "",
      Faculty: "",
      Deparment: "",
      Function: "",
    };
  },
  methods: {
    submit() {
      // Validate all fields
      this.$v.$touch();
      // เช็คว่าในฟอร์มไม่มี error
      if (!this.$v.$invalid) {
        let data = {
          username: this.username,
          password: this.password,
          confirm_password: this.confirm_password,
          first_name: this.first_name,
          last_name: this.last_name,
          email: this.email,
          mobile: this.mobile,
          JobPosition: this.JobPosition,
          address: this.address,
          Faculty: this.Faculty.toUpperCase(),
          Deparment: this.Deparment,
          Function: this.Function,
        };
        axios
          .post("http://localhost:3000/user/signuplibrarian", data)
          .then((res) => {
            this.$router.push("/home");
          })
          .catch((err) => {
            alert(err.response.data.details.message);
          });
      }
    },
  },
  validations: {
    email: {
      required: required,
      email: email,
    },
    password: {
      required: required,
      minLength: minLength(8),
      complex: complexPassword,
    },
    confirm_password: {
      sameAs: sameAs("password"),
    },
    username: {
      required: required,
      minLength: minLength(5),
      maxLength: maxLength(20),
    },
    first_name: {
      required: required,
    },
    last_name: {
      required: required,
    },
    JobPosition: {
      required: required,
    },
    address: {
      required: required,
    },
    mobile: {
      required: required,
      mobile: mobile,
    },
    Faculty: {
      required: required,
    },
    Deparment: {
      required: required,
    },
    Function: {
      required: required,
    },
  },
};
</script>