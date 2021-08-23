<template>
  <div class="wrapper">
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
        <h1>{{ article.title }}</h1>
        <!-- </div> -->
        <p>{{ article.description }}</p>
        <p>Article last updated: {{ formatDate(article.updatedAt) }}</p>

        <nuxt-content :document="article" />

        <author :author="article.author"></author>
        <prev-next :prev="prev" :next="next" />
        <a href="https://www.freepik.com/vectors/flower"
          >Flower vector created by coolvector - www.freepik.com</a
        >
      </div>
    </article>
  </div>
</template>

<script>
import { gsap } from "gsap";
import { ScrollTrigger } from "gsap/ScrollTrigger";

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
  },
  beforeMount() {
    // window.addEventListener("scroll", this.findScrollPosition);
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
  background: rgba(255, 255, 255, 0.767);
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
  /* text-align: center; */
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
  text-shadow: 2px 2px #ffc400;
}

.nuxt-content h4 {
  font-weight: bold;
  font-size: 22px;
  text-shadow: 2px 2px #ffc400;
}

.nuxt .nuxt-content p {
  margin-top: 1rem;
  margin-bottom: 1rem;
}

.nuxt-content-highlight .filename {
  color: pink;
}
</style>
