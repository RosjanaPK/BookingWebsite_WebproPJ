<style>
@import url("https://fonts.googleapis.com/css2?family=Bebas+Neue&display=swap");
body {
  background-image: linear-gradient(
      rgba(220, 130, 26, 0.5),
      rgba(60, 55, 157, 0.5)
    ),
    url("./imageforpage/bgtest.png");
  background-size: cover;
  min-height: 100%;
  width: 100%;
  position: fixed;
  font-family: "Bebas Neue", cursive;
}

.title {
  margin-top: 10%;
}

.box {
  transform: scale(1);
  transition: transform 1s;
  cursor: pointer;
}
.box:hover {
  transform: scale(1.1);
}
</style>
<template>
  <div class="columns">
    <div class="column is-1"></div>
    <div class="column is-10">
      <div class="rows has-text-centered">
        <div class="title">
          <p style="font-size: 45px; color: white; letter-spacing: 10px;">
            WELCOME TO
          </p>
          <p style="font-size: 120px; color: white; letter-spacing: 5px;">
            ONLINE LIBRARY
          </p>
          <p style="font-size: 15px; color: white; letter-spacing: 5px;">
            ALL BOOK YOU NEED IS HERE
          </p>
        </div>
      </div>
      <div class="rows has-text-centered mt-6">
        <div class="columns">
          <div class="column is-2 is-offset-4">
            <div class="box">
              <img
                src="./imageforpage/icon2.png"
                alt=""
                width="100"
                height="100"
              /><br />
              IF(<a style="color: #E6772E">"YOU HAVE ACCOUNT"</a>) <br />
              CLICK HERE
            </div>
          </div>
          <div class="column is-2">
            <div class="box">
              <img
                src="./imageforpage/icon1.png"
                alt=""
                width="100"
                height="100"
              /><br />
              ELSE(<a style="color: #E6772E">"YOU CAN SING UP"</a>)<br />
              CLICK HERE
            </div>
          </div>
        </div>
      </div>
    </div>
    <div class="column is-1"></div>
  </div>
</template>
