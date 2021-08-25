<template>
  <div class="previous-next-container">
    <NuxtLink
      v-if="prev"
      :to="{ name: 'blog-slug', params: { slug: prev.slug } }"
      class="previous-article"
    >
      <span>&larr;</span>
      {{ prev.title }}
    </NuxtLink>
    <span class="previous-article" v-else>&nbsp;</span>
    <NuxtLink
      v-if="next"
      :to="{ name: 'blog-slug', params: { slug: next.slug } }"
      class="next-article"
    >
      {{ next.title }}
      <span>&rarr;</span>
    </NuxtLink>
    <span class="next-article" v-else>&nbsp;</span>
  </div>
</template>

<script>
export default {
  props: {
    prev: {
      type: Object,
      default: () => null
    },
    next: {
      type: Object,
      default: () => null
    }
  }
};
</script>

<style scoped>
.previous-next-container {
  display: flex;
  width: 100%;
  color: orange;
  margin-top: 3rem;
}

.previous-article,
.next-article {
  position: relative;
  width: 50%;
  display: flex;
}

.previous-article {
  justify-content: flex-start;
}

.next-article {
  justify-content: flex-end;
}
</style>
