<template>
  <div class="journal-view" id="iframe" v-if="done">
    <div
      class="blogcard"
      @click.self="openRedirect(blog._id)"
      v-for="blog in journal"
      v-bind:key="blog.title"
    >
      <div v-if="blog._id != exclude">
        <div @click.self="openRedirect(blog._id)" class="hori spce">
          <h2
            @click.self="openRedirect(blog._id)"
            :class="
              this.$root.formatDate(blog.customdate) != blog.title
                ? 'named'
                : ''
            "
          >
            {{ blog.title }}
          </h2>
          <img
            v-if="!blog.ispublic"
            @click="deleteBlog(blog._id)"
            src="@/assets/delete.svg"
            class="del-btn"
          />
          <div v-else class="public-indicator">
            <span class="Mark">
              <svg
                viewBox="0 0 24 24"
                id="ghq-svg-check"
                role="presentation"
                aria-hidden="true"
              >
                <path
                  d="M9.86 18a1 1 0 01-.73-.32l-4.86-5.17a1.001 1.001 0 011.46-1.37l4.12 4.39 8.41-9.2a1 1 0 111.48 1.34l-9.14 10a1 1 0 01-.73.33h-.01z"
                ></path>
              </svg>
            </span>
          </div>
        </div>
        <p
          @click="openRedirect(blog._id)"
          class="date"
          v-if="this.$root.formatDate(blog.customdate) != blog.title"
        >
          {{ this.$root.formatDate(blog.customdate) }}
        </p>
        <div
          @click="openRedirect(blog._id)"
          class="preview"
          v-html="blog.draftbullets[0].data.substring(0, 100)"
        ></div>
        <div
          @click="openRedirect(blog._id)"
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
  props: ["redirect", "blank", "usr_prj_id", "exclude"],
  data() {
    return {
      journal: [],
      done: false,
    };
  },
  components: {},
  async mounted() {
    var usedid = this.usr_prj_id;
    if (!usedid) {
      try {
        usedid = window.location.href.match(/\?[^?]*id=([a-zA-Z0-9]{24})/)[1];
      } catch (e) {
        console.log(e);
      }
    }
    await axios
      .post(vars.url + "/cms/journal", {
        token: VueCookie.get("session_token"),
        usr_prj_id: usedid,
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
    async deleteBlog(bid) {
      await axios
        .post(vars.url + "/cms/delete-blog", {
          token: VueCookie.get("session_token"),
          id: bid,
        })
        .then((response) => {
          if (response.data == "success") {
            for (var i in this.journal) {
              if (this.journal[i]._id == bid) this.journal.splice(i, 1);
            }
          }
        });
    },
  },
};
</script>

<style lang="scss">
.journal-view {
  width: 100%;
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
}

.blogcard {
  cursor: pointer;
  min-width: max(200px, 15.906vw);
  margin: 20px 10px;
  width: min-content;
  @media only screen and (max-width: 530px) {
    width: calc(100% - 30px);
    margin: 20px auto !important;
  }
  box-shadow: 0 0 5px #bbb;
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
img.del-btn {
  width: 12px;

  height: 15px;
  &:hover {
    transform: translateY(-2px);
  }
}

.public-indicator {
  align-items: center;
  background: #0f476f;
  border-radius: 50%;
  display: flex;
  height: 16px;
  width: 16px;
  justify-content: center;
}

// The check mark is optional
.Mark {
  fill: #fff;
  width: 14px;
  height: 20px;
  z-index: 15;
}

p.date {
  color: #444;
  font-size: 10px;
  margin-bottom: 4px;
}
</style>
