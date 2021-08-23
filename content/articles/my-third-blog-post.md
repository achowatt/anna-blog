---
title: My third Blog Post
description: Learning how to use @nuxt/content to create a blog
img: first-blog-post.jpg
alt: my first blog post
author:
  name: Miki
  bio: All about Miki
---

## This is a heading

This is some more info

### This is a sub heading

This is some more info

### This is another sub heading

This is some more info

## This is another heading

This is some more info

Welcome to my first blog post using content module

<div class="p-4 mb-4 text-white bg-blue-500">
  This is HTML inside markdown that has a class of note
</div>

<info-box>
  <template #info-box>
    This is a vue component inside markdown using slots
  </template>
</info-box>

```js
export default {
  nuxt: "is the best"
};
```

```html
<p>code styling is easy</p>
```

```js[my-first-blog-post.md]
export default {
  nuxt: "is the best"
};
```
