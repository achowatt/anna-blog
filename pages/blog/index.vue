<template>
  <main class="blogs-container">
    <header>
      <h1>Blog Posts</h1>
    </header>
    <div class="content-wrapper">
      <div class="filter-search-wrapper">
        <div class="filter-container">
          <h2 @click="toggleFilter" ref="filterRef">Filter</h2>
          <div v-if="showFilter">
            <button v-for="t of articleTopics" :key="t" @click="filter(t)">
              {{ t }}
            </button>
          </div>
        </div>
        <AppSearchInput />
      </div>
      <ul class="posts-container">
        <li v-for="article of filteredArticles" :key="article.slug">
          <NuxtLink
            class="posts"
            :to="{ name: 'blog-slug', params: { slug: article.slug } }"
          >
            <div class="post-image">
              <img :src="article.img" />
            </div>
            <div class="post-details">
              <h2 class="title">{{ article.title }}</h2>
              <p class="description">{{ article.description }}</p>
              <ul class="tag-container">
                <li v-for="(tag, index) in article.tags" :key="index">
                  {{ tag }}
                </li>
              </ul>
            </div>
          </NuxtLink>
        </li>
      </ul>
    </div>
  </main>
</template>

<script>
export default {
  layout: "blog",
  data() {
    return {
      showFilter: false,
      filterChosen: "",
      articleTopics: [
        "javascript",
        "html",
        "css",
        "accessibility",
        "performance",
        "UX/UI",
        "animation",
        "life",
        "no filter"
      ]
    };
  },
  async asyncData({ $content, params }) {
    const articles = await $content("articles")
      .only(["title", "description", "img", "slug", "tags"])
      .sortBy("createdAt", "asc")
      .fetch();

    return {
      articles
    };
  },
  methods: {
    toggleFilter() {
      // if (e.target != this.$refs.filterRef) {
      //   this.showFilter = false;
      //   console.log("yo");
      //   return;
      // }
      this.showFilter = !this.showFilter;
    },
    filter(topic) {
      this.filterChosen = topic;
      let a = this.articles.filter(article =>
        article.tags.some(tag => tag == topic)
      );
      this.toggleFilter(); //close filter
    }
  },
  computed: {
    filteredArticles() {
      if (this.filterChosen == "no filter" || this.filterChosen == "") {
        return this.articles;
      } else {
        return this.articles.filter(article =>
          article.tags.some(tag => tag == this.filterChosen)
        );
      }
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
  min-height: calc(100vh - 7rem);
  margin: 0 auto;
  position: relative;
}

.content-wrapper {
  display: flex;
  flex-direction: column;
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
  background: rgb(255, 77, 77);
  color: white;
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
  background: rgb(255, 207, 207);
}
/* filter mobile */
@media screen and (max-width: 1410px) {
  .content-wrapper {
    display: flex;
    flex-direction: column;
  }

  .filter-search-wrapper {
    display: grid;
    grid-template-columns: 1fr 1fr;
    grid-gap: 2rem;
  }

  .filter-container {
    all: unset;
    width: 100%;
    z-index: 3;
  }

  .filter-container > div {
    background: rgba(253, 253, 253, 0.918);
    /* display: flex;
    align-items: center; */
  }

  .filter-container button {
    background: rgba(253, 253, 253, 0.918);
    border-bottom: solid 1px rgb(255, 223, 163);
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
  font-size: 1.2rem;
  margin: 1rem 0 0 0;
  color: orange;
}

.post-details .description {
  margin-top: 1rem;
  display: flex;
  flex-direction: column;
  justify-content: center;
}

@media screen and (max-width: 915px) {
  .posts-container {
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(250px, 1fr));
  }

  .posts {
    display: flex;
    flex-direction: column;
    height: auto;
  }

  .post-image {
    width: 100%;
  }

  .post-details {
    width: 100%;
    margin-left: 1rem;
  }
}
</style>
