<template>
  <Layout :show-logo="false">
    <!-- Author intro -->
    <Author :show-title="true" />
    <!-- List posts -->
    <div class="posts">
      <PostCard v-for="post in filteredPosts" :key="post.node.id" :post="post.node"/>
    </div>

  </Layout>
</template>

<page-query>
{
  posts: allPost {
    edges {
      node {
        id
        title
        path
        tags {
          id
          title
          path
        }
        date (format: "D. MMMM YYYY")
        timeToRead
        description
        coverImage (width: 770, height: 380, blur: 10)
        ...on Post {
            id
            title
            path
        }
        published
      }
    }
  }
}
</page-query>

<script>
import Author from '~/components/Author.vue'
import PostCard from '~/components/PostCard.vue'

export default {
  components: {
    Author,
    PostCard
  },
  computed: {
    filteredPosts() {
      return this.$page.posts.edges.filter(post => { return post.node.published })
    }
  },
  metaInfo: {
    title: 'Home'
  }
}
</script>
