<script setup lang="ts">
import { ref } from "vue";
import FullPokemon from "./FullPokemon.vue";
import PokemonItem from "@/components/PokemonItem.vue";

const pokemonList = ref([]);
const selectedId = ref<number>(0);
const selectedPokemon = ref();
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

const setSelected = (index: number, pokemon) => {
  selectedId.value = index;
  selectedPokemon.value = pokemon;
}

</script>

<template>
  <div class="grid sm:grid-cols-2">
    <div class="col-span-1 flex flex-col justify-center items-center bg-blue-300 rounded-b-2xl" v-if="selectedId">
      <img :src="`https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/other/official-artwork/${selectedId}.png`" />
      <div>{{ selectedPokemon.name }}</div>
    </div>
    <div v-if="pokemonList" :class="selectedId > 0 ? 'col-span-1 h-screen overflow-y-auto overflow-x-hidden' : 'col-span-2'">
      <div v-for="(pokemon, index) in pokemonList" :key="index">
        <div @click="setSelected(index + 1, pokemon)" v-if="index + 1 < 899" class="py-1">
          <PokemonItem class="w-full hover:scale-105" :name="pokemon.name" :id="index + 1" :class="selectedId === index + 1 ? 'bg-blue-100' : ''">
            <template #image>
              <img :src="`https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/${index + 1}.png`"/>
            </template>
          </PokemonItem>
        </div>
      </div>
    </div>
  </div>
</template>