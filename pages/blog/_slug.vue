<script>
export default {
  async asyncData({ $content, params }) {
    const blogPost = await $content(`articles/${params.slug}`).fetch();

    return {
      blogPost
    };
  }
};
</script>

<template>
  <article>
    <h1>{{ blogPost.title }}</h1>
    <nuxt-content :document="blogPost" />
    <h2>Tags</h2>
    <ul>
      <li v-for="(tag, index) in blogPost.tags" :key="`tag-${index}`">
        <nuxt-link :to="`/?tag=${tag}`">{{ tag }}</nuxt-link>
      </li>
    </ul>
  </article>
</template>

<style></style>
