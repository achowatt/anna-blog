<template>
  <div class="search">
    <div class="result-container">
      <input
        v-model="searchQuery"
        type="search"
        ref="searchBar"
        autocomplete="off"
        placeholder="Search Articles"
        @focus="searching()"
      />
      <ul v-if="articles.length">
        <li v-for="article of articles" :key="article.slug">
          <NuxtLink :to="{ name: 'blog-slug', params: { slug: article.slug } }">
            {{ article.title }}
          </NuxtLink>
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      searchQuery: "",
      articles: [],
      searchFocused: false
    };
  },
  watch: {
    async searchQuery(searchQuery) {
      if (!searchQuery) {
        this.articles = [];
        return;
      }
      this.articles = await this.$content("articles")
        .limit(6)
        .search(searchQuery)
        .fetch();
    }
  },
  methods: {
    searching() {
      this.searchFocused = true;
    }
  }
};
</script>

<style scoped>
ul {
  list-style: none;
  padding: 0.2rem;
  background: white;
}

.search {
  position: absolute;
  right: 0;
  top: 0;
  height: 100vh;
  pointer-events: none;
}

.search input {
  height: 2rem;
  border-radius: 5px;
  border: 1px solid rgb(255, 201, 179);
  padding: 0 1rem;
}

.result-container {
  position: absolute;
  top: 3.5rem;
  right: 1rem;
  pointer-events: auto;
}

.result-container li {
  background: white;
  padding: 0.5rem;
  border-bottom: solid 1px rgb(255, 223, 163);
  transition: background 0.3s;
}

.result-container li:hover {
  background: rgb(255, 223, 163);
  transition: background 0.3s;
}

@media screen and (max-width: 1410px) {
  .search {
    all: unset;
    z-index: 5;
  }

  .search input {
    width: 100%;
    height: 3rem;
  }

  .result-container {
    all: unset;
    /* position: absolute; */
  }
}
</style>
