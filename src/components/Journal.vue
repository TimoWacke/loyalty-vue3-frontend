<template>
  <div class="journal-view" v-if="done">
    <div
      class="blogcard"
      @click="openRedirect(blog._id)"
      v-for="blog in journal"
      v-bind:key="blog.title"
    >
      <h2 :class="this.$root.formatDate(blog.customdate) != blog.title ? 'named' : ''">
        {{ blog.title }}
      </h2>
      <p class="date" v-if="this.$root.formatDate(blog.customdate) != blog.title">
        {{ this.$root.formatDate(blog.customdate) }}
      </p>
      <div
        class="preview"
        v-html="blog.draftbullets[0].data.substring(0, 100)"
      ></div>
      <div
        class="preview"
        v-if="blog.draftbullets[0].data.length < 50 && blog.draftbullets[1]"
        v-html="
          blog.draftbullets[1].data.substring(
            0,
            100 - blog.draftbullets[0].data.length
          )
        "
      ></div>
    </div>
  </div>
</template>

<script>
// @ is an alias to /src

//import axios from "axios";
//import vars from "@/assets/vars";
import VueCookie from "vue-cookies";
import vars from "@/assets/vars";
import axios from "axios";

export default {
  name: "Journal-view",
  props: ["redirect", "blank", "usr_prj_id"],
  data() {
    return {
      journal: [],
      done: false,
    };
  },
  components: {},
  async mounted() {
    await axios
      .post(vars.url + "/cms/journal", {
        token: VueCookie.get("session_token"),
        usr_prj_id: this.usr_prj_id,
      })
      .then((response) => {
        this.journal = response.data;
        this.done = true;
      });
  },
  methods: {
    openRedirect(id) {
      if (this.blank) {
        window.open(this.redirect + "?" + id, "_blank");
      } else {
        window.location.href = this.redirect + "?" + id;
      }
    },
  },
};
</script>

<style lang="scss">
.journal-view {
  width: 100%;
  display: flex;
  flex-wrap: wrap;
  justify-content: stretch;
  align-items: stretch;
}

.blogcard {
  min-width: 250px;
  box-shadow: 0 0 5px #bbb;
  margin: 20px 10px;
  padding: 15px;
  border-radius: 5px;

  h2 {
    margin-top: 0;
    margin-bottom: 12px;
    font-size: 20px;
    &.named {
      margin-bottom: 2px;
      font-size: 16px;
    }
  }

  .preview {
    font-size: 1em;
    margin: 10px 0;
  }
}

p.date {
  color: #444;
  font-size: 10px;
  margin-bottom: 4px;
}
</style>
