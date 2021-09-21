---
title:  Nuxt
description: Syntax or content specific to nuxt
img: https://res.cloudinary.com/djv69vvs7/image/upload/c_scale,w_695/v1629730831/paul-skorupskas-7KLa-xLbSXA-unsplash_ljdhta.jpg
alt: my first blog post
tags:
  - javascript
---

## Vuex
Vuex is managed inside store > index.js
Core Concepts:
- **State:** intializing state values
- **Getters:** returns the state (kind of like computed property where you can filter state without changing the state)
- **Mutations:** change the state value, not asychronous
- **Actions:** allows asynchrous mutations

```js[store/index.js]
export const state = () => ({
  counter: 0,
  list: []
})

export const getters = {
  locale (state) {
    return state.locale
  },
  mainMenuState (state) {
    return state.mainMenu
  },
}

export const mutations = {
  add(state, text) {
    state.list.push({
      text,
      done: false
    })
  },
  remove(state, { todo }) {
    state.list.splice(state.list.indexOf(todo), 1)
  },
  toggle(state, todo) {
    todo.done = !todo.done
  }
}
```

Using Vuex in app
```js 
<script>
import {mapGetters, mapMutations } from 'vuex'

export default {
  computed: {
    todos () { //getter first way
      return this.$store.state.list
    },
    ...mapGetters({ //second way
      todos: 'todos'
    })

  },
  methods: {
    addTodo (e) { //mutate
      this.$store.commit('add', e.target.value)
      e.target.value = ''
    },
    ...mapMutations({
      toggle: 'toggle' //does not need payload 
    }),
    ...mapMutation(['initialize']) //does not need payload 
  }
}
</script>

```

