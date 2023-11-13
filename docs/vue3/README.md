# Vue 3
https://github.com/bencodezen/complete-intro-to-vue-3-workshop

---

## Props

### Passing props

```html
<Character v-for="character in characterList" :name="character.name" @click="favoriteCharacter(character)" />
 ```

 ### Receiving props

```html
<script>
export default {
    props: {
        name: {
            type: String,
            required: true
        },
        click: {
            type: Function,
            required: true
        }
    }
}
</script>

<template>
    <li>
        <p>{{ name }}</p>
        <button @click="click">⭐ Favorite</button>
    </li>
</template>
 ```

 ---

 ## Emitting events

 Emit is a way for children to pass up callbacks to the parent.
 We can pass the label of the event together with some data like this: `$emit('favorite-character', name)`


Full example:

 ```html
 <script>
export default {
    props: {
        name: {
            type: String,
            required: true
        },
        click: {
            type: Function,
            required: true
        }
    },
    emits: ['change-name', 'favorite-character']
}
</script>

<template>
    <li>
        <p>{{ name }}</p>
        <button @click="$emit('favorite-character', name)">⭐ Favorite</button>
        <button @click="$emit('change-name', name)">Change name</button>
    </li>
</template>
```

We can then use it in the parent component like this:

```html
<script>
import Character from './components/Character.vue'
export default {
  components: { Character },
  data: () => ({...}),
  computed: {...},
  methods: {
    favoriteCharacter(name) {
      const isCharacterFavorite = this.favoriteList.find(char => char.name === name)
      if (isCharacterFavorite) return;
      const character = this.characterList.find(char => char.name === name)
      this.favoriteList.push(character)
    },
    changeName(name) {
      this.characterList.find(char => char.name === name).name += '!'
    }
  }
}
</script>

<template>
    // {...}
    <Character :name="character.name" @favorite-character="favoriteCharacter" @change-name="changeName" />
    // {...}
</template>
```

---

## Slots

Slots are a little bit like children in React. Here's a small example of a Button component that can accept children and has a default value of "Click me":

```html
<template>
    <button><slot>Click me</slot></button>
</template>
```

The label click me can then be overrided when Button is used, like so:

```html
<Button>Hello World</Button>
```

We can also use named slots to make layouts components, here's an example:

```html
<template>
    <div class="base-layout">
        <header>
            <slot name="header"></slot>
        </header>
        <main>
            <slot name="main"></slot>
        </main>
        <aside>
            <slot name="aside"></slot>
        </aside>
        <footer>
            <slot name="footer"></slot>
        </footer>
    </div>
</template>

<style>
.base-layout {
    display: grid;
    justify-content: center;
    grid-template-columns: 600px 200px;
    grid-template-rows: 100px 400px 100px;
    grid-template-areas:
        "header header"
        "main aside"
        "footer footer";
}
header {
    grid-area: header;
    background: teal;
}
main {
    grid-area: main;
    background: tomato;
}
aside {
    grid-area: aside;
    background: dodgerblue;
}
footer {
    grid-area: footer;
    background: pink;
}
</style>
```

We can then place the content inside the layout named slots like so:
```html
  <Layout>
    <template v-slot:header>Header</template>
    <template v-slot:main>Main</template>
    <template v-slot:aside>Aside</template>
    <template #footer>Footer</template> // # is a shortcut for v-slot:
  </Layout>
  ```

### Prop slots

We can pass data from the slot up to the parent:

```html
<script setup>
const todoList = ref([])

const completedItems = computed(() => {
  return todoList.value.filter(item => item.completed)
})

const remainingItems = computed(() => {
  return todoList.value.filter(item => !item.completed)
})

function fetchTodoList() {
  fetch('https://jsonplaceholder.typicode.com/todos/')
    .then(response => response.json())
    .then(json => {
      todoList.value = json
    })
}
</script>

<template>
  <div class="section">
    <slot name="hero" />
    <button @click="fetchTodoList">Fetch Data</button>
    <!-- 👇🏼 This is where we pass data ( :completed, :remaining ) up to the parent -->
    <slot
      name="metrics"
      :completed="completedItems"
      :remaining="remainingItems"
    >
      <p>
        {{ completedItems.length }} completed |
        {{ remainingItems.length }} remaining
      </p>
    </slot>
 
  </div>
</template>
```

We can then use the data in the parent like this:

```html
<template>
  <TodoViewer title="This is fun!">
    <template #metrics="propsSlots">
      <h1>Completed: {{ propsSlots.completed.length }}</h1>
      <h2>Remaining: {{ propsSlots.remaining.length }}</h2>
    </template>
  </TodoViewer>
</template>
```


### Modeling slot to parent

We can pass a `_field_` to the slot and then inside the slot we emit `update:_field_`, in this case we have `itemList` passed down and `update:itemList` emitted with some data.

```html
<script setup>
  const props = defineProps({
    itemList: {
      type: Array,
      default: () => []
    },
    itemType: {
      type: String,
      required: true
    }
  })

  const emit = defineEmits(['update:itemList'])

  function fetchItemList() {
    fetch(`https://jsonplaceholder.typicode.com/${props.itemType}`)
      .then(response => response.json())
      .then(json => {
        emit('update:itemList', json)
      })
  }
</script>

<template>
  <!-- Generic Template -->
  <div class="section">
    <button @click="fetchItemList">Fetch Data</button>
    <ul class="list">
      <slot name="items" />
    </ul>
  </div>
</template>
```

In the parent component we can then declare some reactive data with `ref()` and bind it the slot with v-model:

```html
<script setup>
  const photoGallery = ref([])
</script>

<template>
  <BaseDisplay
    itemType="photos"
    v-model:itemList="photoGallery"
  >
    <template v-slot:items>
      <li v-for="photo in photoGallery" :key="`photo-id-${photo.id}`">
        <img :src="photo.thumbnailUrl" />
      </li>
    </template>
  </BaseDisplay>
</template>
```



## Generic Component

We can use the component tag to assign a custom component. Example:

```html
<script>
[...]
export default {
  components: {
    HomePage,
    LoginPage,
    UsersPage
  },
  [...],
  methods: {
    showHomePage() {
      this.currentPage = "Home";
    },
    showLoginPage() {
      this.currentPage = "Login";
    },
    showUserPage() {
      this.currentPage = "Users";
    },
  },
};
</script>

<template>
  [...]
  <component :is="renderPage" />
</template>
```

## Lifecycle Hooks

Just like in React, Vue has got some lifecycled that we can use to run javascript at specific moments during the life cycle of a component.

https://vuejs.org/guide/essentials/lifecycle.html#lifecycle-diagram

To tap into them, we can just use the call the function named as the label in the diagram. Example with the `computed` property:

```html
<script>
export default {
  components: {...},
  data: () => ({
    currentPage: "Home",
  }),
  computed: {
    renderPage() {
      return this.currentPage + 'Page'
    }
  },
  methods: {...},
};
</script>
```

## Composition API

The Composition API was introduced with Vue3.

### Setup

To use the Composition API we can simply add `setup` to the script tag.

```html
<script setup>
  // Composition API Code
</script>
```

### Ref

Ref is a little bit like useState in React, it's a way to declare a piece of reactive data and let Vue handle everything when it changes. To use it we simply import it from the vue library and then call it with `ref()`. To access or manipulate its value we have to get the `.value` property on it. ( A little bit like `ref.current` in React )

```html
<script setup>
  import { ref } from 'vue'
  const count = ref(0)

  function increment() {
    count.value++
  }
</script>
```

### defineProps and defineEmits

These two functions are global in the Composition API, we don't need to import them like `ref`.

```html
<script setup>
  const props = defineProps({
    suffix: {
      type: String,
    }
  })

  const emit = defineEmits(['done'])

  emit('done', { status: true });
</script>

<template>
  <main>
    <h1>Users {{ suffix }}</h1>
  </main>
</template>

```

### Composables

These are the equivalent of custom hook in react. It's a way of storing data in a module and being able to consume it in different places. As a standard these javascript files are all put inside a folder `/composables`.

Example `/src/composables/useCount.js`:

```js
import { ref } from "vue";

const globalCount = ref(0);

export const useCount = () => {
  const localCount = ref(0);

  function increment() {
    localCount.value++;
  }

  function decrement() {
    localCount.value -= 1;
  }

  function incrementGlobal() {
    globalCount.value++;
  }

  function decrementGlobal() {
    globalCount.value -= 1;
  }

  return {
    localCount,
    globalCount,
    increment,
    decrement,
    incrementGlobal,
    decrementGlobal,
  };
};
```

After we declare the hook, we can use it in any place in our app:

```html
<script setup>
import { useCount } from "../composables/useCount";

const { incrementGlobal } = useCount()

defineProps({
  user: {
    type: Object,
    required: true,
  }
})
</script>

<template>
  <li class="user-card">{{ user.name }}: {{ user.website }}
    <button @click="incrementGlobal">+</button>
  </li>
</template>
```

## Routing

Similarly to React, Vue also has a library to deal with routing: https://router.vuejs.org/

We need to setup the router before mounting the app:

```js
  // Please note that usually files for pages are put inside a views folder
  import HomePage from "./views/HomePage.vue";
  import LoginPage from "./views/LoginPage.vue";
  import UserPage from "./views/UsersPage.vue";
  const router = createRouter({
    history: createWebHashHistory(),
    routes: [
      {
        path: "/",
        component: HomePage,
      },
      {
        path: "/login",
        component: LoginPage,
      },
      {
        path: "/users",
        component: UserPage,
      },
      {
        path: "/dashboard",
        component: () => import("./views/DashboardPage.vue"),
      },
      {
        path: "/user/:id",
        component: () => import("./views/UserPage.vue"),
      },
    ],
  });

  createApp(App).use(router).mount("#app");
```

In the App component we can then use `RouterView` to display the content and `RouterLink` to navigate to other pages:

```html
  <template>
    <header class="header">
      <span class="logo">
        <img src="@/assets/vue-heart.png" width="30" />C'est La Vue
      </span>
      <nav class="nav">
        <RouterLink to="/">Home</RouterLink>
        <RouterLink to="/login">Login</RouterLink>
        <RouterLink to="/users">Users</RouterLink>
      </nav>
    </header>
    <Suspense>
      <RouterView />
      <template v-slot:fallback>Loading...</template>
    </Suspense>
  </template>
```

### Programmatic navigation

We can use the `useRouter` hook to navigate to other pages with JS:

```js
  import { useRouter } from 'vue-router';

  const router = useRouter()
  
  function onClick() {
    router.push('/dashboard')
  }
```

### Dynamic routing

We can use params such as `:id` when setting up the router to navigate to dynamic routes. 

We can then use the hook `useRoute` to read these params from the page with the dynamic route:

```html
<script setup>
import { useRoute } from 'vue-router';


const { params } = useRoute()

const data = await fetch(`https://jsonplaceholder.typicode.com/users/${params.id}`).then(res => res.json())

</script>

<template>
    <main>
        <h1>User {{ params.id }}</h1>
        <pre>{{ data }}</pre>
    </main>
</template>
```

## Pinia for state management

Pinia is a library for state management, just like what Redux is ( was ☠️ ) for React. https://pinia.vuejs.org/

Just like vue-router, this needs to be setup before mounting the app:

```js
import { createPinia } from "pinia";
import App from "./App.vue";

const pinia = createPinia();

createApp(App).use(pinia).mount("#app");
```

Then inside a folder named `stores` ( conventional naming ) we can declare custom hooks that defines slices of store using `defineStore`, this takes two parameters, first one is the key, second one is an options object with three keys: `state`, `getters` and `actions`:

```js
import { defineStore } from "pinia";

export const useUsersStore = defineStore("UsersStore", {
  // Data
  state: () => ({
    users: [],
  }),
  // Computed properties to get pieces of state
  getters: {
    getUserById: (state) => {
      return (id) => state.users.find((user) => user.id == id);
    },
    getShortUserList: (state) => state.users.splice(0, 4),
  },
  // Methods to modify state
  actions: {
    fetchUsers() {
      fetch("https://jsonplaceholder.typicode.com/users")
        .then((response) => response.json())
        .then((res) => {
          this.users = res;
        });
    },
  },
});
```

When can then use the hook inside a custom component: 
N.B. Best to avoid destructuring things, it fucks things up with Vue.

```html
<script setup>
import { useRoute } from 'vue-router';
import { useUsersStore } from '../stores/UsersStore';

const { params } = useRoute()
const store = useUsersStore()

const data = store.getUserById(params.id)
</script>

<template>
    <main>
        <h1>User {{ params.id }}</h1>
        <pre>{{ store.getUserById(params.id) }}</pre>
    </main>
</template>
```

## Useful libraries

VueUse: Library of util functions - https://vueuse.org/ 
TransitionGroup Component: https://vuejs.org/guide/built-ins/transition-group.html#transitiongroup

## Best practices

### Naming conventions

Avoid single worded names: Header, Button, Container

Instead prefix them with `App`, `Base` or `The`: `TheHeader`, `BaseButton`, `AppContainer`

Name methods something meaningful:  
❌ `onInput`  
✅ `updateUsername`

Prefer destructuring over multiple arguments:  
❌ `updateUsername(userList, index, isActive, isFocused) {`  
✅ `updateUsername({ userList, index, isActive, isFocused }) {`

### Refactoring

You might need refactor when:
- The components are hard to understand
- You feel a part of the component should have its own state
- It's difficult to say what the component does exactly

# Nuxt 3

`NuxtPage`: to show the page content ( can be used to render parts of a page too )

`NuxtLink`: to link to other pages

`useRoute`: to get route info `const route = useRoute()`

`NuxtLayout`: to use layouts inside layout dir

Definition `layouts/breadcrumb.vue`:
```html
<script lang="ts" setup></script>
<template>
  <div>
    <p>This is the Breadcrumb</p>
    <slot />
  </div>
</template>
```

Usage `pages/index.vue`:
```html
<script lang="ts" setup></script>
<template>
 <NuxtLayout name="breadcrumb">
   <p>This is the content of page 2</p>
   <NuxtLink to="/">Go back to index</NuxtLink>
 </NuxtLayout>
</template>
```