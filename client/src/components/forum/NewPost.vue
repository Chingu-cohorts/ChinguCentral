<template>
<div>
  <section class="hero is-dark is-bold">
    <div class="hero-body">
      <div class="container">
        <h1 class="title">New Post</h1>
        <h2 class="subtitle">Share your new great idea</h2>
      </div>
    </div>
  </section>

  <div class="container new-post">
    <div class="columns">
      <div class="column is-8 is-offset-2">
        <div class="field">
          <label class="label">Title</label>
          <p class="control">
            <input class="input" type="text" v-model="post.title">
          </p>
        </div>

        <markdown-editor v-model="post.content" ref="markdownEditor"></markdown-editor>

        <div class="field is-grouped">
          <p class="control">
            <button class="button is-primary is-outlined" @click="savePost">Submit</button>
          </p>
          <p class="control">
            <router-link
              :to="{ name: 'PostList' }"
              class="button is-danger is-outlined">Cancel</router-link>
          </p>
        </div>
      </div>
    </div>

  </div>
</div>
</template>

<script>
import { markdownEditor } from 'vue-simplemde'
import { http } from '@/api'

export default {
  name: 'NewPost',

  data () {
    return {
      post: {
        title: '',
        content: ''
      }
    }
  },

  components: {
    markdownEditor
  },

  computed: {
    loggedUser () {
      return this.$store.state.user.loggedUser
    }
  },

  methods: {
    savePost (e) {
      let { title, content } = this.post

      let post = {
        title,
        content
      }

      http.post('/posts', {
        title: post.title,
        content: post.content
      }).then(res => {
        this.$router.push({ name: 'PostList' })
      }).catch(err => {
        if (err.response) {
          console.log(err.response.data)
        } else {
          console.error(err)
        }
      })
    }
  },

  mounted () {
    if (!this.loggedUser.username) {
      this.$router.push({ name: 'SignIn' })
    }
  }
}
</script>

<style scoped>
.new-post {
  margin-top: 2em;
  margin-bottom: 2em;
}

@media screen and (max-width: 768px) {
  .new-post {
    margin-left: 1em;
    margin-right: 1em;
  }
}
</style>
