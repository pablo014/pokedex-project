<script setup lang="ts">
import { ref } from "vue";
import FullPokemon from "./FullPokemon.vue";
import PokemonItem from "@/components/PokemonItem.vue";

const pokemonList = ref([]);
const imageUrl = ""
const selectedId = ref<number>(0);
let req = new Request('https://pokeapi.co/api/v2/pokemon?limit=100000&offset=0');
fetch(req).then((res) => {
  if(res.status === 200)
    return res.json();
})
.then((data) => {
  data.results.forEach(pokemon => {
    pokemonList.value.push(pokemon);
  })
})
.catch((err) => {
  console.error(err)
})



</script>

<template>
  <div class="grid grid-cols-2">
    <div v-if="pokemonList" :class="selectedId > 0 ? 'col-span-1 h-screen overflow-auto' : 'col-span-2'">
      <div v-for="(pokemon, index) in pokemonList" :key="index">
        <div @click="selectedId = index + 1" v-if="index + 1 < 899" class="py-1">
          <PokemonItem class="w-full" :name="pokemon.name" :id="index + 1">
            <template #image>
              <img :src="`https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/${index + 1}.png`"/>
            </template>
          </PokemonItem>
        </div>
      </div>
    </div>
    <div class="col-span-1" v-if="selectedId">
      <FullPokemon :id="selectedId"/>
    </div>
  </div>
</template>