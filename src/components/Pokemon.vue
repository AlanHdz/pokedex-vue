<template>
  <div class="row">
    <transition name="fade">
      <div class="pokemon" :key="pokemonId">
        <div class="pokemon-body">
          <h3 class="text-center pokemon-name">
            #{{ pokemonId }} {{ pokemon.name }}
          </h3>
          <img :key="pokemonId" :src="pokemonImage" class="poke-img" />
          <p class="text-center top-space">
            <span
              class="types"
              :class="type.type.name"
              v-for="(type, index) in pokemonTypes"
              :key="index"
              >{{ type.type.name }}</span
            >
          </p>
        </div>
      </div>
    </transition>
  </div>
</template>

<script>
import axios from "axios";
import { ref, onMounted, watch, toRefs } from "vue";
export default {
  props: {
    pokemon: {
      type: Object,
      required: true,
    },
  },
  setup(props) {
    let { pokemon } = toRefs(props);
    let isLoading = ref(false);
    let pokemonId = ref(0);
    let pokemonImage = ref("");
    let pokemonTypes = ref([]);

    const getPokemon = async () => {
      isLoading.value = true;
      let response = await axios.get(
        `https://pokeapi.co/api/v2/pokemon/${pokemon.value.name}`
      );
      pokemonId.value = response.data.id;
      pokemonImage.value =
        response.data.sprites.other["official-artwork"].front_default;
      pokemonTypes.value = response.data.types;
      isLoading.value = false;
    };

    onMounted(() => {
      getPokemon();
    });

    watch(pokemon, getPokemon);

    return {
      pokemonId,
      pokemonImage,
      pokemonTypes,
      isLoading,
      getPokemon,
    };
  },
};
</script>

<style scoped>
.row {
  width: 33%;
  height: 350px;
  transition: 0.3s;
  margin-top: 1em;
}
.top-space {
  margin-top: 1em;
}
h3 {
  margin-top: 1em;
}
.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.5s ease;
}

.fade-enter-from,
.fade-leave-to {
  opacity: 0;
}
.pokemon-name {
  margin-bottom: 1em;
  text-transform: uppercase;
  font-weight: 500;
}
.pokemon {
  width: 70%;
  height: 100%;
  margin: 0 auto;
  box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2);
  border: 3px solid #FFC107;
  color: #343A40;
}
.pokemon .poke-img {
  width: 70%;
  display: block;
  height: 200px;
  margin: 0 auto;
}
.pokemon .pokemon-body {
  padding: 10px 15px;
}
.types {
  color: white;
  border-radius: 10px;
  padding: 3px 5px;
  text-transform: uppercase;
  margin-right: 2px;
}
.text-center {
  text-align: center;
}
.normal {
  background-color: #a8a77a;
}
.fire {
  background-color: #ee8130;
}
.water {
  background-color: #6390f0;
}
.electric {
  background-color: #f7d02c;
}
.grass {
  background-color: #7ac74c;
}
.ice {
  background-color: #96d9d6;
}
.fighting {
  background-color: #c22e28;
}
.poison {
  background-color: #a33ea1;
}
.ground {
  background-color: #e2bf65;
}
.flying {
  background-color: #a98ff3;
}
.psychic {
  background-color: #f95587;
}
.bug {
  background-color: #a6b91a;
}
.rock {
  background-color: #a6b91a;
}
.ghost {
  background-color: #735797;
}
.dragon {
  background-color: #6f35fc;
}
.dark {
  background-color: #705746;
}
.steel {
  background-color: #b7b7ce;
}
.fairy {
  background-color: #d685ad;
}
</style>
