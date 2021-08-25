<template>
  <div :class="[searchFocused ? 'focus' : '', 'search']">
    <div class="result-container">
      <input
        v-model="searchQuery"
        type="search"
        ref="searchBar"
        autocomplete="off"
        placeholder="Search Articles"
        @focus="searching()"
        @blur="stopSearching()"
      />
      <ul :class="[searchFocused ? 'focus' : '']" v-if="articles.length">
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
      this.$refs.searchBar.style.transform = "scale(2)";
      this.$refs.searchBar.style.transition = "all 0.3s";
      this.$refs.searchBar.style.transformOrigin = "100% 50%";
    },
    stopSearching() {
      this.searchFocused = false;
      this.$refs.searchBar.style.transform = "scale(1)";
      this.searchQuery = "";
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
  width: 100%;
  height: 100vh;
  pointer-events: none;
}

.search.focus {
  background: rgba(255, 255, 255, 0.616);
  pointer-events: auto;
}

.search input {
  height: 2rem;
}

.result-container {
  position: absolute;
  top: 3.5rem;
  right: 1rem;
  width: 10rem;
  pointer-events: auto;
}

.result-container li {
  background: white;
  padding: 1rem;
  border-radius: 3px;
  transition: background 0.3s;
}

.result-container li:hover {
  background: rgb(255, 223, 163);
  transition: background 0.3s;
}

.result-container ul.focus {
  transform: scale(2);
  transform-origin: 100% 0%;
}
</style>
