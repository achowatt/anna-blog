---
title: Vue 3 Quick Syntax Look Up
description: Quick reminders for people who have experience with Vue. Not recommended for beginners.
img: https://res.cloudinary.com/djv69vvs7/image/upload/c_scale,w_695/v1629730831/paul-skorupskas-7KLa-xLbSXA-unsplash_ljdhta.jpg
alt: my first blog post
author:
  name: Anna Chowattanakul
  bio: Front End Developer
---

## Vue Life Cycles

created() - data and computed are available?

## Directives

v-if

```vue
<h1 v-if="awesome">Vue is awesome!</h1>
```

v-else-if

```vue
<h1 v-else-if="superawesome">Vue is super awesome!</h1>
```

v-else

```vue
<h1 v-else>Oh no 😢</h1>
```

v-show

```vue
//CSS based toggle
<h1 v-show="ok">Hello!</h1>
```

v-for

```vue
//Loop over an array []
<li v-for="todo in todos" :key="todo.id"> {{todo.text }}</li>
//Loop over an object and its keys {key: "value"}
<li
  v-for="(value, key, index) in myBook"
  :key="value.id"
> {{key}} : {{value}} </li>
//Loop over a range -- outputs 1 2 3 4 5...20
<li v-for="n in 20"> {{n}} </li>
```

v-text

```vue
<span v-text="msg"></span>
<!-- same as -->
<span>{{msg}}</span>
```

v-html

```vue
<div v-html="html"></div>
```

```vue
v-slot


v-on     @click|@event
<button @click="counter++">Press Me </button>


v-bind   :href|:[key]|:id
<div :id="dynamicId></div>
<button :disabled="isButtonDisabled">Button</button>

v-model


v-pre
<span v-pre>{{will not compile}}</span>


v-cloak


v-once
<span v-once> This will never change: {{msg}} </span>


v-memo


v-is


```

## Modifiers

```vue
v-model.trim .number .lazy
```

## Props

Passing props to component:

```vue
<example-component status="pass" /> //passing a string
<example-component :status="pass" /> //you have to bind to use a variable
```

Inside component that's receiving the props

```js
// This is only OK when prototyping
props: ['status']

//Possible Prop Types:
String, Number, Boolean, Array, Object, Date, Function, Symbol

props: {
  title: String,
  likes: Number,
  isPublished: Boolean,
  commentIds: Array,
  author: Object,
  callback: Function,
  contactsPromise: Promise // or any other constructor
}

// Prop validations
props: {
 status: String,
 propA: [String, Number], // Multiple possible types
 propB: {type: String, required: true}, //required string
 propD: {type: Number, default: 100} //default
 propD: {type: Object,
         default() {
           return { message: 'hello' } //default for object or array
         }
        },
 propE: {
 	   validator(value) {
            return ['success', 'warning', 'danger'].includes(value). //Custom Validator (props must be one of these?)
           }
         },
 propG: {
	 type: Function,
	 default() {
	  return 'Default function' //Function with default value
         }
        }

}
```

## Emit

- Pass props back from child to parent during an event

```vue[child-component.vue]
<template>
  <button @click="addGenre(genre)">{{ genre }}</button>
  <template>
    <script>
      export default {
        props: ["genre"]
        emits: ['addGenre','removeGenre],  //register
        methods: {
           addGenre(genre) {
             this.$emit("addGenre", genre);  //1st param: function name. 2nd param: prop
           }
        }
      }
    </script></template
  ></template
>
```

```vue[parent-component.vue]
<template>
  <div v-for...>
    <GenreButtons :genre="genre" @addGenre="addGenre(genre)" //event />
  </div>

  <script>
    export default {
      ....
      methods: {
       addGenre(genre) { //complete action in parent
         this.genre.push(genre)
       }

      }
    }
  </script>
</template>
```

## Computed Properties

- Must return a value
- Good for changing the presentation of data (without changing the data)
- Cached. Only re-render when the dependencies change.
- Use case: filtering data based on user input, etc

```vue
computed: { filteredMovies() { let filter = new RegExp(this.findMovie, 'i');
//the dependency here is "this.findMovie" return this.movies.filter(el =>
el.match(filter)); }, reversedItems() { return [...this.items].reverse() //the
dependency here is "this.items" } }
```

```vue
//Using computed properties
<li v-for="movie in filteredMovies">{{movie}}</li>
```

## Watchers

- Is a side effect (does not return a value, but can update something else)
- Triggers based on only one dependencies whereas computed properties can have multiple dependencies
- Access to old and new value

```vue
Example 1: data() { return { counter: '', score: '', } }, watch: {
counter(newVal, oldVal) { console.log(`old value ${oldVal}, new value
${newVal}`); } score() { console.log("Score Updated") this.image="" } }
```

```vue
Example 2: Combining watch and create() in the same hook. (Use handler gets
triggered when created + when watcher changes) watch: { score: { handler:
"updateImage", immediate: true } }, method: { updateImage() { this.image = "" }
```

## Class and Style bindings

```vue
CLASS
<div :class= "[uiState === "lost" ? "frown" : "", item.highPriority ? "priority": "","cls3"]></div> //combining different conditionals
<h1 :class= "{active: isActive, priority: item.highProprity}"> Hello </h1>. //active = class name, isActive = Boolean
<h1 :class= "{'text-danger': isActive}">Hello </h1> //put " " around class name with -

STYLE
<h2 :style="{color : activeColor, fontSize: size}"> Hello There</h2>. //color = css property, activeColor = data()

<h2 :style="myStyles">Hello</h2>
data() {
 return {
  myStyles: {
   fontSize: '100px',
   color: 'blue'
  }
 }
}
```

## Local vs Global Components

- global components is registered at the app level, and can be used anywhere

```vue
const app = Vue.createApp({}) app.component('SearchInput', SearchInputComponent)
```

- local components has to be registered within the file

```vue
import ComponentA from './ComponentA.vue' import ComponentB from
'./ComponentB.vue' export default { components: { ComponentA, ComponentB } //
... }
```

## Slot

- Allows you to pass content into the child component without using props
- You can name the slots pass in multiple elements and keep track

```vue[parent.vue]
<template>
  <example-child-component>
    <h3 slot="header">Hello World!</h3>
    <p>Bonjour!</p>
  </example-child-component>
</template>
```

```vue[example-child-component.vue]
<template>
  <div>
    <slot name="header"></slot>
    <slot></slot>
  </div>
</template>
```

## Scoped Slot

- allows you to pass data() from child component to parent component

```vue[parent.vue]
<template>
  <h1>hello</h1>
  <todo-comp>
    //adding child component
    <div slot-scope="{ todo }">
      <span v-if="todo.done"> ✓ </span> //conditionally rendering data based on
      the todo data() from child component
      {{ todo.name }}
    </div>
  </todo-comp>
</template>
```

```vue[child-component.vue]
<template>
  <div>
    <h1>My todo List</h1>
    <ul>
      <li v-for="todo in todos" :key="todo.name">
        <slot :todo="todo"> //fallback content {{ todo.name }} </slot>
      </li>
    </ul>
  </div>
</template>

<script>
 export default {
  data() {
   return {
     todos: [
{ name: "Fix computer", done: true },
       { name: "Take out the garbage", done: false}
     ]
   }
 }
</script>
```

## Dynamic Components

```vue
<component :is="dynamicComp"></component> //render component based on the
variable //Add keep-alive to preserve the state of the component & Avoid
rerender //So if the component gets toggled, the component will not revert to
its original state
<!-- basic -->
<keep-alive>
  <component :is="view"></component>
</keep-alive>

<!-- multiple conditional children -->
<keep-alive>
  <comp-a v-if="a > 1"></comp-a>
  <comp-b v-else></comp-b>
</keep-alive>

<!-- used together with `<transition>` -->
<transition>
  <keep-alive>
    <component :is="view"></component>
  </keep-alive>
</transition>
```

## Transition/ Animations

- transitions for rendering v-if, v-show, dynamic components, component root nodes

```vue
<transition name="my-transition"></transition>

CSS classes: * v-enter * v-enter-active /* durations and timing functions ex.
transition: all .3s ease; * v-enter-to * v-leave * v-leave-active /* durations
and timing functions. transition: all .8s cubic-bezier(1.0, 0.5, 0.8, 1.0); *
v-leave-to
```

Example 1: Fade in and out

```vue
<button v-on:click="show = !show">
  Toggle
</button>
<transition name="fade">
  <p v-if="show">hello</p>
</transition>
```

```css
.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.5s;
}
.fade-enter, .fade-leave-to /* .fade-leave-active below version 2.1.8 */ {
  opacity: 0;
}
```

Example 2: Using Keyframe

```vue
<button v-on:click="show = !show">
  Toggle
</button>
<transition name="bounce">
  <p v-if="show">hello</p>
</transition>
```

```css
.bounce-enter-active {
  animation: bounce-in 0.5s;
}
.bounce-leave-active {
  animation: bounce-in 0.5s reverse;
}
@keyframes bounce-in {
  0% {
    transform: translateX(20px);
    /* Enter from / leave to */
  }
  50% {
    transform: translateX(-30px);
  }
  100% {
    transform: translateX(0);
  }
}
```

Example 3: This way is good for adding custom classes (css libraries)

```vue
<button v-on:click="show = !show">
  Toggle
</button>
<transition enter-active-class="bounce" leave-active-class="bounce-out">
  <h1 v-if="show">Hello</h1>
</transition>
```

```css
.bounce {
  /* shorter class name */
  animation: bounce-in 0.5s;
}
.bounce-out {
  animation: bounce-in 0.5s reverse;
}

@keyframes bounce-in {
  0% {
    transform: translateX(20px);
    /* Enter from / leave to */
  }
  50% {
    transform: translateX(-30px);
  }
  100% {
    transform: translateX(0);
  }
}
```

## Mixins

- A block of reusable code
- A mixin object can contain any component options.
- When a component uses a mixin, all options in the mixin will be "mixed" into the component's own options.
- Can get a bit confusing if there are more than one mixin used for a component

```js
const myMixin = {
  //mixin object
  created() {
    console.log("mixin hook called");
  }
};

const app = Vue.createApp({
  mixins: [myMixin],
  created() {
    console.log("component hook called");
  }
});

// return
// => "mixin hook called"
// => "component hook called"
```

Global Mixin

```js
Vue.mixin({
  create() {
    console.log("global mixin");
  }
});
```

## Creating a custom directive

Coming soon

## Testing

Unit Testing with Jest https://vue-test-utils.vuejs.org/guides/#getting-started
E2E Testing Cypress

## Router

Basic Router

```js[router/index.js]
import Home from "@/views/Home.vue" //register page to path const routes = [ {
path:"/" name:"Home", component:Home } ]
```

Code Splitting (don't load the page before navigating)

```js[router/index.js]
const routes = [
  {
    path: "/musicians",
    name: "Musicians",
    component: () => import("@/views/Musicians.vue")
  }
];
```

Navigation Guard for dynamic route

```js[router/index.js]
const routes = [
  {
    path: "/concerts/:id/:slug",
    name: "concert.details",
    component: () => import("@/views/ConcertDetails.vue"),
    beforeEnter(to, from) {
      //navigation guard to check if the page exists
      const exists = getConcert(to.params.id);
      if (!exists)
        return {
          name: "NotFound",
          params: {
            pathMatch: to.path.split("/").slice(1)
          },
          query: to.query,
          hash: to.hash
        };
    }
  }
];
```

Start on top of the page every time the route chages

```js[router/index.js]
const router = createRouter({
  history: createWebHistory(),
  routes,
  scrollBehavior(to, from, savedPosition) {
    //scroll to top on page changes return
    savedPosition ||
      new Promise(resolve => {
        setTimeout(() => resolve({ top: 0 }), 1200);
      });
  }
});
```

Changing router based on an event

```vue
<button @click="onPressed()">Go Home </button>

methods: { onPressed() { this.$router.push("/"); } }
```
