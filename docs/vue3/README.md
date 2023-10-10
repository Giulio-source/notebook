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