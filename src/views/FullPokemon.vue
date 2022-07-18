<script setup lang="ts">
import { defineProps, withDefaults, computed } from "vue"

interface Props {
  id?: Number;
}

const props = withDefaults(defineProps<Props>(), {
  id: 0,
})

const pokemon = computed(() => {
let pokemonItem = {};
if (props.id) {
let req = new Request(`https://pokeapi.co/api/v2/pokemon/${props.id}`)
fetch(req).then((res) => {
  if (res.statusCode == 200)
    return res.json()
})
.then((data) => {
  pokemonItem = data
})
.catch((err) => { console.error(err) })
}
return pokemonItem;
})
</script>

<template>
  {{ pokemon }}
  <div v-if="pokemon">
    {{ pokemon.name }}
  </div>
</template>