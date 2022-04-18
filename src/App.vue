<template>
  <div><router-view /></div>
</template>

<script>
export default {
  store: {},
  mounted() {
    document.addEventListener("keyup", function (event) {
      // Number 13 is the "Enter" key on the keyboard
      if (event.keyCode === 13 && !event.shiftKey) {
        // Trigger the button element with a click
        document.getElementById("send").click();
      }
    });
  },
  methods: {
    formatDate(date, format) {
      var dat = new Date(date);
      if (format) {
        format = format.replace("YYYY", dat.getFullYear());
        format = format.replace("YY", dat.getYear());
        format = format.replace(
          "MM",
          (dat.getMonth() + 1 < 10 ? "0" : "") + (dat.getMonth() + 1)
        );
        format = format.replace("M", dat.getMonth() + 1);
        format = format.replace(
          "DD",
          (dat.getDate() < 10 ? "0" : "") + dat.getDate()
        );
        format = format.replace("D", dat.getDate());
        return format;
      }
      const months = [
        "January",
        "February",
        "March",
        "April",
        "May",
        "June",
        "July",
        "August",
        "September",
        "October",
        "November",
        "December",
      ];
      const day = dat.getDate();
      var sib = "th";
      if (day < 4) {
        sib = "rd";
        if (day < 3) sib = "nd";
        if (day == 1) sib = "st";
      }
      return day + sib + " " + months[dat.getMonth()] + " " + dat.getFullYear();
    },
  },
};
</script>



<style lang="scss">
$primary: #0f476f;
$secondary: #ff049b;

.sign-page {
  margin: 0;
  height: 80vh;
  display: flex;
  align-items: center;
}

#content,
.sign-page {
  font-family: "Segoe UI";
  color: $primary;
}

.none {
  display: none;
}

.pointer {
  cursor: pointer;
}

#content.site {
  max-width: calc(100% - 120px);
  display: flex;
  justify-content: center;
  padding: 100px 60px 50px 60px;
  margin: 0;
  .wrap {
    min-width: min-content;
  }
}

#content {
  margin: 120px 60px 120px 0;
  @media only screen and (min-width: 1700px) {
    padding-right: 50px;
  }
  padding-right: calc(5vw - 100px);
  display: flex;
  flex-direction: column;
  justify-content: center;

  .wrap {
    display: flex;

    .project {
      margin-right: 50px;
      min-width: 25vw;
      margin-bottom: auto;
    }
  }
}

h1,
h3 {
  margin: 0px 0 15px;
}

.tight:is(h1, h2, h3) {
  margin: 5px 0;
}

.headers {
  h1,
  h2,
  h3,
  h4 {
    margin-top: 10px;
    margin-bottom: 10px;
    border-radius: 5px;
  }
  h2 {
    padding: 5px 30px 5px 15px;
    color: #fff;
    font-size: 16px;
    background: $secondary;
    @media only screen and (min-width: 800px) {
      background: linear-gradient(120deg, #f0b0 15%, $secondary 20%);
      margin-left: calc(-3vw - 100px);
      padding-left: calc(3vw + 100px);
    }
  }
  h1 {
    @media only screen and (max-width: 800px) {
      font-size: 24px;
    }
  }
  margin-bottom: 50px;
}

.bborder {
  border: 1px solid #000;
  border-radius: 3px;
  background: transparent;
  outline: none;
  padding: 4px;
}

p,
a {
  font-family: "Segoe UI";
  margin: 0;
  color: $primary;

  @media only screen and (max-height: 800px) {
    &.mobile-white {
      color: #fff;
    }
  }
}

#error {
  color: $secondary;
}
.hidden {
  opacity: 0;
  height: 0;
}

hr.hidden {
  width: 6px;
}

.card {
  padding: 15px;
  margin: 15px auto;
  display: flex;
  width: calc(100% - 30px);
  flex-direction: column;
  justify-content: center;
  background: $primary;
  border-radius: 4px;
  color: #fff;

  input {
    width: calc(100% - 20px);
  }

  div {
    align-self: flex-start;
    text-align: left;
    p,
    a,
    button {
      margin: 5px 0;
      font-size: 16px;
    }
    &.small {
      margin: 5px 12px 10px;
      p,
      a {
        font-size: 12px;
      }
      #error {
        margin-bottom: 5px;
      }
    }
  }

  h2 {
    font-size: 20px;
    align-self: flex-start;
    margin: 8px 0 16px;
  }

  p,
  a {
    color: #fff;
  }

  button {
    background: $secondary;
    color: #fff;
    width: 36%;

    @media only screen and (max-width: 900px) {
      width: 50%;
    }
    @media only screen and (max-width: 600px) {
      width: 100%;
    }
    min-width: 100px;
    &.right {
      margin-top: calc(10px - 1.5vw);
    }
  }
}

.column,
form {
  display: flex;
  flex-direction: column;
}

.right {
  margin-left: auto;
}

.hori {
  display: flex;
  flex-direction: row;
  padding: 0;
  &.zontal {
    align-items: center;
    input,
    select {
      width: 100%;
    }
  }
  &.spce {
    width: 100%;
    justify-content: space-between;
  }

  &.mobile-cmpr {
    @media only screen and (max-width: 900px) {
      flex-direction: column;

      hr,
      p {
        display: none;
      }
    }
  }
  &.d {
    @media only screen and (max-width: 900px) {
      flex-direction: column;
    }
  }
}

.shadow {
  box-shadow: 0px 0px 4px 0px #ddd;
  border-radius: 8px;
  padding: 0 15px;
}

.container {
  display: flex;
  flex-direction: column;
  align-items: flex-start;
  width: 61%;
  margin: auto;

  @media only screen and (max-width: 800px) {
    width: calc(80% + 30px);
  }

  h1 {
    color: $primary;
  }
  &.tight {
    @media only screen and (min-width: 1200px) {
      width: 50%;
    }

    @media only screen and (min-width: 1650px) {
      width: 40%;
    }
  }
}

div:empty {
  display: none;
}

label {
  margin-top: 10px;
  font-weight: 600;
}
input,
button,
select {
  margin: 5px 0px;
  border: none;
  outline: none;
  &:not([type="color"]) {
    padding: 5px 10px;
  }
  border-radius: 3px;
  white-space: nowrap;
  &:disabled {
    opacity: 0.69;
    cursor: default;
  }
}

button {
  color: #fff;
  cursor: pointer;
  font-weight: bold;
  padding: 5px 10px;
  background-color: $primary;
}

.gradient {
  background: linear-gradient(
    20deg,
    rgba(8, 36, 56, 1) 0%,
    rgba(15, 71, 111, 1) 100%
  );
  a,
  * {
    color: #fff;
  }
}

body {
  margin: 0;
}

hr {
  width: 2px;
  height: 64px;
  background: #fff;
  outline: none;
  border: none;
  margin: 0 5px;
}

#frame {
  position: absolute;
  display: flex;
  align-items: flex-end;
  justify-content: center;
  top: 0;
  padding: 0;
  margin: 0;
  height: 100vh;
  width: 100vw;
  overflow: hidden;
  z-index: -1;

  #footer-img {
    width: 100%;
    min-width: 1200px;
    margin-bottom: -15vw;
  }
}

select:not([type="color"]) {
  padding: 4px 8px;
}

#vlogo {
  @media only screen and (min-width: 1700px) {
    position: absolute;
    top: 45px;
    right: 300px;
    width: 210px !important;
  }

  @media only screen and (min-width: 900px) {
    width: calc(150px + 10vw);
    margin: auto calc(-75px - 5vw) calc(5vw - 75px) auto;
  }
  @media only screen and (max-width: 900px) {
    margin: 0 0 0 auto;
  }
}

.content-icon {
  margin-top: 30px;
  height: 30px;
}

.file {
  label {
    display: none;
  }
  .content-icon {
    margin: auto 0 auto 0;
  }
  margin: 10px auto;
}

.pushdown {
  height: 100%;
  display: flex;
  align-items: flex-end;
  button {
    min-width: 100px;
  }
}

.public-layout {
  margin-top: 20px;
  width: 250px;
  p {
    margin-top: 10px;
  }
}

.node {
  display: flex;
  flex-direction: row;
}

#content #details {
  display: flex;
  flex-direction: row !important;
  justify-content: space-between;
  #details {
    margin: 10px;
    label {
      margin-bottom: 10px;
    }
  }

  .field {
    margin: 0px 10px;
    .text-right {
      text-align: right;
    }
  }
}

#content .home #details {
  display: flex;
  flex-direction: column !important;
  #details {
    margin: 0 !important;
  }
  .box {
    .box {
      flex-direction: row !important;
    }
  }
}

.project {
  width: min-content;
}

#content #details #details {
  display: flex;
  flex-wrap: wrap;
  width: auto;
  .field {
    min-width: max-content;
    p {
      max-width: 200px;
      white-space: wrap;
    }
  }
}

#details .box {
  display: flex;

  margin-left: 10px;

  input {
    border: 1px solid #000;
    border-radius: 2px;
    background: transparent;
  }
  .box {
    justify-content: space-between;
    flex-direction: row;
    .field {
      padding-left: 10px;
      p {
        min-width: 100px;
        max-width: 150px;
      }
    }
  }
  .field {
    display: flex;
    flex-direction: column;
  }

  textarea {
    min-height: 50px;
    max-height: calc(15vh + 50px);
    min-width: calc(25vw - 10px);
    max-width: calc(25vw - 10px);
    padding: 5px;
  }

  .draft button {
    margin-bottom: 30px;
    display: flex;
  }
}

@media only screen and (max-width: 800px) {
  .hori.highlvl {
    flex-direction: column;
  }
  #menubtn {
    right: 10px !important;
    position: absolute !important;
    margin: 0;
    padding: 0;
  }

  #content.site {
    max-width: calc(100vw - 30px);
    padding: 100px 15px 50px 15px;
    .wrap {
      flex-direction: column;
      width: 100%;
    }
    .project {
      width: calc(100vw - 30px - 30px);
      overflow: hidden;
      padding: 5px 15px 10px;
      margin: 20px 0;

      h2,
      h3,
      h4 {
        font-size: 14px;
        a {
          font-size: 14px;
        }
        margin: 10px 0 5px;
      }
      font-size: 8px;
      p,
      a,
      label,
      input,
      button,
      select,
      textarea {
        font-size: 10px;
      }
      hr {
        height: 16px;
      }
      .mobile-compr {
        flex-wrap: wrap;
      }
      margin-bottom: 0;
      .dragdrop,
      textarea {
        max-width: calc(100% - 22px) !important;
        min-width: calc(100% - 22px) !important;

        padding: 5px 10px;
      }
      .node:not(.file) {
        margin-left: 0;
        margin-right: 0;

        #details {
          flex-direction: column !important;
          margin-left: 0 !important;
          margin-right: 0 !important;
          div {
            margin-left: 0;
            margin-right: 0;
            div {
              margin-left: 0;
              margin-right: 0;
            }
          }
        }
      }
      .content-icon img {
        width: 20px !important;
        height: 20px !important;
        margin: 5px 10px 0 0;
      }
      button {
        min-width: 66px;
      }
    }
  }
}
</style>
