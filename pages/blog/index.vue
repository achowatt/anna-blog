<template>
  <main class="blogs-container">
    <header>
      <h1>Blog Posts</h1>
      <AppSearchInput />
    </header>
    <div class="content-wrapper">
      <div class="filter-container active">
        <h2>Filter By Topics</h2>
        <button>Javascript</button>
        <button>Html</button>
        <button>CSS</button>
        <button>Accessibity</button>
        <button>Performance</button>
        <button>UX/UI</button>
        <button>Animation</button>
        <button>Life</button>
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
  async asyncData({ $content, params }) {
    const articles = await $content("articles")
      .only(["title", "description", "img", "slug", "author"])
      .sortBy("createdAt", "asc")
      .fetch();

    return {
      articles
    };
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
  width: 100%;
  /* padding: 0 2rem 0 0; */
  margin-top: 1rem;
  text-align: center;
  position: fixed;
  right: calc(((100vw - 1200px) / 2) + 1200px);
}

.filter-container.active {
  max-width: 100%;
  right: 0;
  top: 0;
  margin-top: 0;
  transform: translateX(0);
  background: white;
  display: flex;
  align-items: center;
  overflow-x: scroll;
}

.filter-container h2 {
  background: white;
  padding: 0.5rem;
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
