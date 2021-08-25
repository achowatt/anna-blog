<template>
  <main class="blogs-container">
    <header>
      <h1>Blog Posts</h1>
      <!-- <button class="filter-button" @click="toggleFilter()">
        filter by topics
      </button> -->
      <AppSearchInput />
    </header>
    <div class="content-wrapper">
      <div class="filter-container">
        <h2 @click="toggleFilter">Filter</h2>
        <transition name="fade">
          <div v-show="showFilter">
            <button>Javascript</button>
            <button>Html</button>
            <button>CSS</button>
            <button>Accessibity</button>
            <button>Performance</button>
            <button>UX/UI</button>
            <button>Animation</button>
            <button>Life</button>
            <button>All</button>
          </div>
        </transition>
      </div>
      <ul class="posts-container">
        <li v-for="article of articles" :key="article.slug">
          <NuxtLink
            class="posts"
            :to="{ name: 'blog-slug', params: { slug: article.slug } }"
          >
            <div class="post-image">
              <img :src="article.img" />
            </div>
            <div class="post-details">
              <h2 class="title">{{ article.title }}</h2>
              <p class="author">by {{ article.author.name }}</p>
              <p class="description">{{ article.description }}</p>
            </div>
          </NuxtLink>
        </li>
      </ul>
    </div>
  </main>
</template>

<script>
export default {
  data() {
    return {
      showFilter: false
    };
  },
  async asyncData({ $content, params }) {
    const articles = await $content("articles")
      .only(["title", "description", "img", "slug", "author"])
      .sortBy("createdAt", "asc")
      .fetch();

    return {
      articles
    };
  },
  methods: {
    toggleFilter() {
      this.showFilter = !this.showFilter;
    }
  }
};
</script>

<style scoped>
ul {
  list-style: none;
}

header {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.5s ease;
}

.fade-enter-from,
.fade-leave-to {
  opacity: 0;
}

.blogs-container {
  padding: 1rem;
  max-width: 1200px;
  margin: 0 auto;
  position: relative;
}

.content-wrapper {
  display: flex;
}

.filter-container {
  max-width: 15rem;
  height: 7rem;
  margin-top: 1rem;
  margin-left: 1rem;
  text-align: center;
  position: fixed;
  right: calc(((100vw - 1200px) / 2) + 1200px);
}

.filter-container h2 {
  padding: 0.5rem;
  color: orange;
  background: white;
  border-radius: 5px;
  box-shadow: 5px 5px 10px rgba(255, 236, 215, 0.678);
  cursor: pointer;
}

.filter-container button {
  border: none;
  width: 100%;
  padding: 0.7rem;
  cursor: pointer;
  background: rgba(255, 255, 255, 0.479);
}

.filter-container button:hover {
  background: rgb(255, 223, 163);
}

@media screen and (max-width: 1410px) {
  .filter-container {
    z-index: 3;
    max-width: unset;
    width: 100%;
    height: unset;
    left: 0;
    bottom: 0;
    margin-left: 0;
    margin-top: 0;
    /* transform: translateX(0); */
    /* background: rgba(255, 255, 255, 0.699); */
    /* border: 1px solid black; */
    /* display: none; */
    overflow-x: scroll;
    display: flex;
  }

  .filter-container > div {
    display: flex;
    align-items: center;
  }

  .filter-container button {
    background: rgba(253, 253, 253, 0.918);
  }
}

.posts-container {
  list-style: none;
  display: grid;
  grid-template-columns: 1fr 1fr;
  grid-gap: 2rem;
  padding: 0;
  width: 100%;
}

.posts {
  display: flex;
  padding: 1rem;
  box-shadow: 5px 5px 5px rgba(182, 182, 182, 0.438);
  border-radius: 5px;
  background: rgba(255, 255, 255, 0.664);
  overflow: hidden;
  height: 16rem;
}

.post-image {
  width: 50%;
  border-top-left-radius: 5px;
  border-bottom-left-radius: 5px;
  overflow: hidden;
}

.posts:hover img {
  transform: scale(2);
  transition: transform 0.3s;
}

.posts img {
  transition: transform 0.3s;
  width: 100%;
  height: 100%;
}

.post-details {
  width: 50%;
  margin-left: 1rem;
}

.post-details .title {
  font-size: 2rem;
  margin: 1rem 0 0 0;
  color: orange;
}

.post-details .author {
}

.post-details .description {
  margin-top: 1rem;
}
</style>
