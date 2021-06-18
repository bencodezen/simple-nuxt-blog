<template>
  <div class="container">
    <div class="inner-container">
      <h1>My Blog</h1>
      <h2>Recent Blogs</h2>
      <p v-if="filterTag">
        Filtered by: {{ filterTag }}
        <button @click="clearFilter">Clear filter</button>
      </p>
      <ul>
        <li v-for="post in filteredPosts" :key="post.slug">
          <nuxt-link :to="`/blog/${post.slug}`">
            <h3>{{ post.title }}</h3>
          </nuxt-link>
          <p>by {{ post.author }}</p>
          <p>{{ post.description }}</p>
          <ul>
            <li v-for="tag in post.tags" :key="`${post.slug}-tag-${tag}`">
              <button @click="setTag(tag)">{{ tag }}</button>
            </li>
          </ul>
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
export default {
  async asyncData({ $content }) {
    const articles = await $content("articles").fetch();

    return {
      articles
    };
  },
  data: () => ({
    filterTag: ""
  }),
  computed: {
    filteredPosts() {
      if (this.filterTag) {
        return this.articles.filter(article => {
          return article.tags.includes(this.filterTag);
        });
      } else {
        return this.articles;
      }
    }
  },
  methods: {
    clearFilter() {
      this.filterTag = "";
      this.updateLocation();
    },
    updateLocation(query) {
      this.$router.push({
        path: this.$route.path,
        query
      });
    },
    setTag(tag) {
      this.filterTag = tag;
      this.updateLocation({ tag });
    }
  },
  mounted() {
    this.filterTag = this.$route.query.tag;
  }
};
</script>

<style>
.container {
  margin: 0 auto;
  min-height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
}

.title {
  font-family: "Quicksand", "Source Sans Pro", -apple-system, BlinkMacSystemFont,
    "Segoe UI", Roboto, "Helvetica Neue", Arial, sans-serif;
  display: block;
  font-weight: 300;
  font-size: 100px;
  color: #35495e;
  letter-spacing: 1px;
}

.subtitle {
  font-weight: 300;
  font-size: 42px;
  color: #526488;
  word-spacing: 5px;
  padding-bottom: 15px;
}

.links {
  padding-top: 15px;
}
</style>
