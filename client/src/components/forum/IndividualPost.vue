<template>
<div class="box">
  <div class="columns">
    <article class="media">
      <div class="column is-3-mobile is-2-desktop">
        <div class="media-left">
          <figure class="image is-64x64">
            <img :src="userGravatar" class="avatar" :alt="post.user.username">
          </figure>
        </div>
      </div>

      <div class="column is-5-mobile is-8-desktop">
        <div class="media-content">
          <div class="content">
            <router-link
              :to="{name: 'ShowPost', params: { id: post.id, slug: postSlug }}">{{ post.title }}
              </router-link> <small>by <strong>{{ post.user.username }}</strong></small> <small>{{ timeAgo }}</small>
            <br>
            <p class="is-hidden-mobile">{{ truncatePost }}</p>
          </div>
        </div>
      </div>

      <div class="column is-4-mobile is-2-desktop">
        <div class="media-right has-text-centered">
          <h2>{{ commentsCount }}</h2>
          <span>Comments</span>
        </div>
      </div>
    </article>
  </div>
</div>
</template>

<script>
import moment from 'moment'
import { gravatar } from '@/utils'

export default {
  name: 'IndividualPost',

  props: ['post'],

  computed: {
    userGravatar (props) {
      let { email } = props.post.user
      return gravatar(email)
    },

    timeAgo (props) {
      let date = new Date(props.post.updated_at)
      return moment(date, 'YYYYMMDD').fromNow()
    },

    postSlug (props) {
      let { slug } = props.post
      return slug
    },

    commentsCount (props) {
      if (props.post.comments) {
        return props.post.comments.length
      }
      return '0'
    },

    truncatePost (props) {
      let { post } = props
      let num = 130

      if (post.content.length <= num) return post.content
      return post.content.slice(0, num - 3) + '...'
    }
  }
}
</script>

<style scoped>
figure {
  margin: 0 auto;
}

.media {
  width: 100%;
}

.media-right h2 {
  font-weight: 700;
}
</style>
