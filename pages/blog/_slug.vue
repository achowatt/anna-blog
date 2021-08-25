<template>
  <main class="wrapper">
    <article class="blog-article">
      <BikeScrollBar />

      <nav class="blog-nav">
        <ul>
          <h4>On this page</h4>
          <li class="quick-links" v-for="link of article.toc" :key="link.id">
            <NuxtLink :to="`#${link.id}`">{{ link.text }}</NuxtLink>
          </li>
        </ul>
      </nav>

      <div class="blog-content">
        <!-- <img :src="article.img" :alt="article.alt" /> -->
        <!-- <div
          class="blog-img"
          :style="{ backgroundImage: `url(${article.img})` }"
        > -->
        <!-- </div> -->
        <h1>{{ article.title }}</h1>
        <p>{{ article.description }}</p>
        <p>Article last updated: {{ formatDate(article.updatedAt) }}</p>

        <nuxt-content :document="article" />

        <prev-next :prev="prev" :next="next" />
      </div>
    </article>
  </main>
</template>

<script>
import { gsap } from "gsap";

export default {
  layout: "blog",
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
  },
  mounted() {
    gsap.set(".blog-nav", { xPercent: -100 });
    gsap.to(".blog-nav", 1.5, { xPercent: 0 });
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
  min-height: 100vh;
}

.blog-nav {
  order: 2;
  width: 17rem;
  background: rgba(255, 255, 255, 0.507);
}

.blog-nav ul {
  position: sticky;
  top: 4rem;
  left: 0;
  list-style: none;
}

.blog-nav li {
  margin: 0.5rem 0;
  font-family: poiret-one, sans-serif;
}

.blog-nav h4 {
  font-weight: bold;
  color: transparent;
  -webkit-text-stroke-width: 1px;
  -webkit-text-stroke-color: rgba(247, 101, 4, 0.712);
  font-family: broadacre-hairline-4, sans-serif;
  font-size: 1.5rem;
}

.blog-content {
  width: calc(100% - 17rem); /* minus width of nav */
  max-width: 1100px;
  box-shadow: 5px 5px 15px rgba(173, 172, 172, 0.507);
  padding: 2rem;
  background: rgb(255, 255, 255);
  z-index: 2;
}

.blog-img {
  width: 100%;
  height: 50vh;
  background-repeat: no-repeat;
  background-position-y: 50%;
  background-size: cover;

  text-align: center;
  display: flex;
  align-items: flex-end;
  justify-content: center;
  color: white;
}

.blog-content h1 {
  text-shadow: 2px 2px #ffc400;
}

.blog-content img {
  width: 100%;
  height: 30vh;
  object-fit: cover;
  border-radius: 5px;
}

.blog-article .nuxt-link-exact-active {
  color: red;
  position: relative;
}

.blog-article .nuxt-link-exact-active::before {
  content: "";
  width: 20px;
  height: 2px;
  background: red;
  position: absolute;
  bottom: 6px;
  left: -25px;
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
  margin-top: 3rem;
  /* color: #ffc400; */
  font-family: broadacre-hairline-4, sans-serif;
  -webkit-text-stroke-width: 1px;
  -webkit-text-stroke-color: #ffc400;
  color: #ffc40067;
  /* text-shadow: 2px 2px #ffc400; */
}

.nuxt-content h4 {
  font-weight: bold;
  font-size: 22px;
  text-shadow: 2px 2px #ffc400;
}

.nuxt-content-highlight {
  position: relative;
}

.nuxt-content-highlight .filename {
  color: rgba(15, 252, 201, 0.836);
  position: absolute;
  right: 5px;
  bottom: 5px;
  font-size: 0.8rem;
}
</style>
