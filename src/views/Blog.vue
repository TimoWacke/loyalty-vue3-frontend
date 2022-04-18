<template>
  <div class="hori highlvl">
    <Dashboard />
    <div v-if="done" id="content" class="site blog">
      <h3>What's on your mind {{ aps["user"].edit.forename }}?</h3>
      <br />
      <div id="editor">
        <input
          class="h2"
          type="text"
          id="blogtitle"
          v-model="blog.title"
          v-autowidth="{
            maxWidth: '100%',
            minWidth: '0px',
            comfortZone: 0,
          }"
        />

        <div
          class="bullet"
          @click.self="setFocus(b)"
          v-for="(bullet, b) in blog.draftbullets"
          v-bind:key="b"
        >
          <div class="hori">
            <div class="dot-wrp">
              <div class="dot"></div>
            </div>
            <contenteditable
              :id="'li-' + b"
              class="content-wrp"
              tag="div"
              :contenteditable="blog.draftbullets[b].isEditable"
              @input="syncInner(b)"
              :noHtml="true"
              @keydown.tab="checkTab(b, $event)"
              @keydown.backspace="checkBackspace(b)"
              @paste="checkPaste($event)"
            />
          </div>
          <hr class="underline" />
        </div>
      </div>
      <div id="addbullet" class="plus bullet" @click="newBullet(-1)">
        <div class="hori">
          <div class="dot-wrp">
            <div class="dot"></div>
          </div>
        </div>
      </div>

      <Saver class="blog" :toserve="[blog]" :nodisplay="true" />

      <button class="inputnone" id="send"></button>
      <div id="divider" @click="toggleDivider()">
        <div id="upbutton">
          <hr id="line1" />
          <hr id="line2" />
        </div>
        <hr class="breakline" />
      </div>
    </div>
  </div>
</template>

<script>
// @ is an alias to /src

//import axios from "axios";
//import vars from "@/assets/vars";
import App from "@/App.vue";
import vars from "@/assets/vars";
import VueCookie from "vue-cookies";
import Dashboard from "@/components/Dashboard.vue";
import Saver from "@/components/Saver.vue";

import axios from "axios";
export default {
  name: "Blog-Site",
  data() {
    return {
      aps: App.store,
      blog: {},
      done: false,
    };
  },
  components: {
    Dashboard,
    Saver,
  },
  async mounted() {
    await axios
      .post(vars.url + "/cms/blog", {
        token: VueCookie.get("session_token"),
      })
      .then((response) => {
        this.blog = response.data;
        this.blog["edit"] = true;
        if (this.blog.draftbullets.length == 0) {
          this.blog.draftbullets = [{ data: "", isEditable: true }];
        }
        this.done = true;
        setTimeout(this.initialise, 50);
      })
      .catch((error) => {
        console.log(error);
      });
  },
  methods: {
    syncInner(b) {
      this.blog.draftbullets[b].data = document.getElementById(
        "li-" + b
      ).innerHTML;
      document.getElementById("addbullet").classList.remove("compr");
      if (
        this.blog.draftbullets[this.blog.draftbullets.length - 1].data == ""
      ) {
        document.getElementById("addbullet").classList.add("compr");
      }
    },
    initialise() {
      for (var b in this.blog.draftbullets) {
        document.getElementById("li-" + b).innerHTML =
          this.blog.draftbullets[b].data;
      }
      document.getElementById("addbullet").classList.remove("compr");
      if (
        this.blog.draftbullets[this.blog.draftbullets.length - 1].data == ""
      ) {
        document.getElementById("addbullet").classList.add("compr");
      }
    },
    checkTab(b, e) {
      console.log("check on bullet b:", b);
      if (e.shiftKey) {
        if (b > 0) {
          this.setFocus(b);
        }
        return;
      }
      if (this.blog.draftbullets.length - 1 == b) {
        this.newBullet(b + 1);
      } else if (this.blog.draftbullets[b + 1].data != "") {
        console.log("new bullet");
        this.newBullet(b + 1);
      }
    },
    checkBackspace(b) {
      if (this.blog.draftbullets[b].data == "" && b > 0) {
        this.deleteBullet(b);
      }
    },
    newBullet(b) {
      if (b == -1) {
        b = this.blog.draftbullets.length;
      }
      this.blog.draftbullets.splice(b, 0, { data: "", isEditable: true });
      setTimeout(this.initialise, 25);
      setTimeout(this.setFocus(b), 100);
    },
    deleteBullet(b) {
      this.blog.draftbullets.splice(b, 1);
      setTimeout(this.initialise, 25);
      setTimeout(this.setFocus(b - 1), 50);
    },
    setFocus(b) {
      var bullet = document.getElementById("li-" + b);
      bullet.focus();

},
    toggleDivider() {
      document.getElementById("divider").classList.toggle("flipped");
      document.getElementById("editor").classList.toggle("compr");
      document.getElementById("addbullet").classList.toggle("compr");
    },
    checkPaste(e) {
      // cancel paste
      e.preventDefault();

      // get text representation of clipboard
      var text = (e.originalEvent || e).clipboardData.getData("text/plain");

      // insert text manually
      document.execCommand("insertHTML", true, text);
    },
  },
};
</script>

<style lang="scss">
#save {
  width: 100px;
  margin-bottom: -30px;
}

#cursor {
  width: 0;
}

#upbutton {
  height: 15px;
  position: relative;
  hr {
    background-color: #777;
    height: 27px;
    width: 4px;
    border-radius: 4px;
    position: absolute;

    &#line1 {
      transform: rotate(45deg) translateX(-12px);
    }
    &#line2 {
      transform: rotate(-45deg) translateX(12px);
    }
  }
}

.compr .bullet {
  display: none;
}

#blogtitle {
  padding: 0;
}

#divider {
  margin-top: -50px;
  &.flipped {
    margin-top: -80px;
    transform: scaleY(-1);
  }
  display: flex;
  flex-direction: column;
  align-items: center;
  width: 100%;
  transition: all 0.5s ease;
  padding: 20px 0;
  &:hover {
    &:not(.flipped) {
      transform: translateY(-10px);
    }
    &.flipped {
      transform: scaleY(-1) translateY(-10px);
    }
  }
  cursor: pointer;
}

.inputnone {
  position: fixed;
  opacity: 0;
  bottom: 0;
  right: 0;
  z-index: -10;
}
#content.blog {
  display: flex;
  width: 100%;
}

input.h2 {
  font-size: 24px;
  margin: 0 0 25px;
  font-weight: 600;
  color: #0f476f;
}

hr.underline {
  width: calc(100% - 20px);
  height: 1px;
  margin: 10px 5px 5px;
  background-color: #eee;
}

hr.breakline {
  width: calc(100%);
  background-color: #777;
  border-radius: 65%;
  height: 4px;
  margin: 10px 0;
}

.bullet {
  min-height: 50px !important;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  div.dot-wrp,
  div.content-wrp {
    min-height: 21px;
  }
  div.dot-wrp {
    width: 20px;
    display: flex;
    justify-content: center;
    align-items: center;
    div.dot {
      display: flex;
      background-color: #0f476f;
      height: 10px;
      width: 10px;
      width: 10px;
      border-radius: 50%;
      z-index: 3;
    }
  }

  .content-wrp {
    display: inline;
    margin-left: 15px;
    font-size: 20px;
    line-height: 24px;
    vertical-align: baseline;
    width: calc(100% - 40px);
    outline: none;
  }
}

div.plus.bullet {
  cursor: pointer;
  margin-top: -5px !important;
  margin-bottom: -30px !important;
  opacity: 40%;
  &.compr {
    display: none;
  }
  @media (hover: hover) and (pointer: fine) {
    display: none;
  }
}

div.bullet {
  margin: 20px 0 30px !important;
}
@keyframes blink-animation {
  to {
    visibility: hidden;
  }
}
@-webkit-keyframes blink-animation {
  to {
    visibility: hidden;
  }
}
</style>
