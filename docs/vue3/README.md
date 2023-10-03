# Vue 3
https://github.com/bencodezen/complete-intro-to-vue-3-workshop

## Props

### Passing props:

```html
<Character v-for="character in characterList" :name="character.name" @click="favoriteCharacter(character)" />
 ```

 ### Receiving props:

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