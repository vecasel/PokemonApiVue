<script setup>
  import {ref, onMounted, computed} from 'vue';
  import LoadingPage from './components/LoadingPage.vue';
  import PokeTitle from './components/PokeTitle.vue';
  import PokemonList from './components/PokemonList.vue';
  import PokemonsButton from './components/PokemonsButton.vue';
  import PaginatePokemon from './components/PaginatePokemon.vue';
   
  const loadingSpinner = ref(true);
  const pokeApi = ref({});
  const pokeList = ref([]);
  const showPokemonButton = ref(true);

  onMounted(async () => {
    await loadingPage();
  });

  const loadingPage = (async(pokeApiUrl = 'https://pokeapi.co/api/v2/pokemon/') =>{
    try{
      const res = await fetch(pokeApiUrl);
      pokeApi.value = await res.json();
    }
    catch(error){
      console.log(error);
    }
    finally{
      pokeList.value = pokeApi.value.results;
      setTimeout(()=>{
        loadingSpinner.value = false;
      }, 2000);
    }
  });

  const nextValue = computed(()=>{
    return pokeApi.value.next;
  });

  const previousValue = computed(()=>{
    return pokeApi.value.previous;
  });

  const pokeLength = computed(()=>{
    return pokeList.value.length;
  });

  const showList = () => {
    showPokemonButton.value = false;
  }


  console.log(pokeApi);
  console.log(nextValue);
  console.log(previousValue);
  console.log(pokeLength);

</script>

<template>
  <div class="loading"  v-if="loadingSpinner">
    <LoadingPage></LoadingPage> 
  </div>
  <div v-else>
    <header class="header">
      <PokeTitle></PokeTitle>
    </header>
    <main class="main">
      <h2>Cantidad de Pokemon: {{pokeApi.count}}</h2>
      <PokemonsButton 
        v-if="showPokemonButton && pokeLength > 0"
        @showPokemon="showList"
      >
      </PokemonsButton>
      <div class="main__container" v-else>
        <div class="main__container--buttonsP">
          <PaginatePokemon
            :next="nextValue"
            :previous="previousValue"
            @loadingPage="loadingPage"
          >
          </PaginatePokemon>
        </div>
        <div  class="main__container--list">
          <PokemonList
            v-for="pokemon in pokeList"
            :key="pokemon.name"
            :name="pokemon.name"
            :url="pokemon.url" 
          >
          </PokemonList>
        </div>
      </div>
    </main>
  </div>
</template>