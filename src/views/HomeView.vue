<script setup lang="ts">
import PokeCard from "@/components/PokeCard.vue";
import { api } from "@/services/api";
import { onMounted, ref } from "vue";

interface Pokemon {
  name: string;
  url: string;
}

const pokemons = ref([] as Pokemon[]);
const fetchPokemons = () =>
  api
    .get("pokemon?limit=151")
    .then((res) => (pokemons.value = res.data.results));
onMounted(fetchPokemons);

let search: "";

const filteredPokemons = ref([] as Pokemon[]);

async function onChange(event: any) {
  search = event.target.value;

  await api
    .get("pokemon?limit=151")
    .then((res) => (filteredPokemons.value = res.data.results));

  const filtered = filteredPokemons.value.filter((item) =>
    item.name.startsWith(search)
  );

  filteredPokemons.value = filtered;
  console.log(filtered);
}
</script>

<template>
  <header>
    <input
      class="searchInput"
      type="text"
      placeholder="Search"
      v-on:input="onChange($event)"
      v-model="search"
    />
  </header>
  <main>
    <PokeCard
      v-if="filteredPokemons.length > 0"
      v-for="(filteredPokemon, idx) in filteredPokemons"
      :key="idx"
      :name="filteredPokemon.name"
      :id="idx + 1"
      :image="`https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/${
        idx + 1
      }.png`"
    />

    <h1 v-else-if="search !== '' && filteredPokemons.length === 0">
      Oops... this name doesn`t exists in our Pokedex, sorry!
    </h1>

    <PokeCard
      v-else
      v-for="(pokemon, i) in pokemons"
      :key="i"
      :name="pokemon.name"
      :id="i + 1"
      :image="`https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/${
        i + 1
      }.png`"
    />
  </main>
</template>

<style lang="scss">
header {
  display: flex;
  padding: 8px;

  .searchInput {
    flex: 1;
    background: transparent;
    border: transparent;
    border-bottom: 1px solid #c2c2c2;
  }
}

main {
  display: flex;
  flex-wrap: wrap;
  align-items: center;

  gap: 8px;
}
</style>
