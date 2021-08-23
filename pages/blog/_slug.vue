<template>
  <div class="wrapper">
    <article class="blog-article">
      <BikeScrollBar />

      <nav class="blog-nav">
        <ul>
          <h4>Quick Review</h4>
          <li v-for="link of article.toc" :key="link.id">
            <NuxtLink :to="`#${link.id}`">{{ link.text }}</NuxtLink>
          </li>
        </ul>
      </nav>

      <div class="blog-content">
        <h1>{{ article.title }}</h1>
        <p>{{ article.description }}</p>

        <img :src="article.img" :alt="article.alt" />
        <p>Article last updated: {{ formatDate(article.updatedAt) }}</p>

        <nuxt-content :document="article" />

        <author :author="article.author"></author>
        <prev-next :prev="prev" :next="next" />
      </div>
    </article>
  </div>
</template>

<script>
export default {
  async asyncData({ $content, params }) {
    const article = await $content("articles", params.slug).fetch();

    const [prev, next] = await $content("articles")
      .only(["title", "slug"])
      .sortBy("createdAt", "asc")
      .surround(params.slug)
      .fetch();

    return {
      article,
      prev,
      next
    };
  },
  methods: {
    formatDate(date) {
      const options = { year: "numeric", month: "long", day: "numeric" };
      return new Date(date).toLocaleDateString("en", options);
    }
  }
};
</script>

<style>
::-webkit-scrollbar {
  width: 0;
  /* Remove scrollbar space */
}

.blog-article {
  margin-left: clamp(17.5vh, 15%, 500px); /* min,preferred,max */
  display: flex;
  padding: 1rem;
}

.blog-nav {
  order: 2;
  width: 17rem;
}

.blog-nav ul {
  position: sticky;
  top: 3rem;
  left: 0;
  list-style: none;
}

.blog-content {
  width: calc(100% - 17rem); /* minus width of nav */
  max-width: 1000px;
}

@media screen and (max-width: 1565px) {
  /* .blog-nav {
    width: 40%;
  }
  .blog-content {
    width: 60%;
  } */
}

.nuxt-content h2 {
  font-weight: bold;
  font-size: 28px;
}

.nuxt-content h3 {
  font-weight: bold;
  font-size: 22px;
}

.nuxt-content p {
  margin-bottom: 20px;
}

.nuxt-content-highlight .filename {
  color: pink;
}
</style>
