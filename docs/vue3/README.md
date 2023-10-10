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