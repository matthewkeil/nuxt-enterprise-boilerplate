<script lang="ts">
import { Component, Vue, Watch } from 'vue-property-decorator'
@Component({
  components: {
    BlogFeedCard: () => import('@/components/blog/BlogFeedCard.vue')
  }
})
export default class BlogFeed extends Vue {
  private layout = [2, 2, 1, 2, 2, 3, 3, 3, 3, 3, 3]
  private page = 1

  @Watch('page') onPageChage() {
    window.scrollTo(0, 0)
  }

  get articles() {
    return this.$vuex.blog.posts
  }

  get pages() {
    return Math.ceil(this.articles.length / 11)
  }

  get paginatedArticles() {
    const start = (this.page - 1) * 11
    const stop = this.page * 11
    return this.articles.slice(start, stop)
  }
}
</script>

<template>
  <v-container>
    <v-row>
      <v-col cols="12">
        <slot />
      </v-col>

      <blog-feed-card
        v-for="(article, i) in paginatedArticles"
        :key="article.title"
        :size="layout[i]"
        :value="article"
      />
    </v-row>

    <v-row align="center">
      <v-col cols="3">
        <base-button
          v-if="page !== 1"
          class="ml-0"
          square
          title="Previous page"
          @click="page--"
        >
          <v-icon>mdi-chevron-left</v-icon>
        </base-button>
      </v-col>

      <v-col class="text-center subheading" cols="6">
        PAGE {{ page }} OF {{ pages }}
      </v-col>

      <v-col class="text-right" cols="3">
        <base-button
          v-if="pages > 1 && page < pages"
          class="mr-0"
          square
          title="Next page"
          @click="page++"
        >
          <v-icon>mdi-chevron-right</v-icon>
        </base-button>
      </v-col>
    </v-row>
  </v-container>
</template>
