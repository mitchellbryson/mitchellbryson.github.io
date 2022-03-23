<template>
  <div>
    <h1>{{ page.title }}</h1>
    <p>{{ page.description }}</p>
    <nuxt-content :document="page" />
  </div>
</template>

<script lang="ts">
import Vue from 'vue'
import { MetaInfo } from 'vue-meta'

export default Vue.extend({
  async asyncData({ $content, params, error }) {
    const slug = params.slug || 'index'
    const page = await $content(slug)
      .fetch()
      .catch(() => {
        error({ statusCode: 404, message: 'Page not found' })
      })

    return {
      page,
    }
  },
  data(): { page: { title: string } } {
    return {
      page: {
        title: '',
      },
    }
  },
  head(): MetaInfo {
    return {
      title: this.page.title,
    }
  },
})
</script>
