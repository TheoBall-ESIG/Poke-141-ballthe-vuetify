<template>
  <!-- Conteneur principal pour structurer la disposition de la page -->
  <v-container>
    <!--
    Titre de la page
      * text-h3 applique le style titre de niveau 3
      * text-center permet de centrer le texte
      * my-6 ajoute une marge verticale de 6 unités
    -->
    <h1 class="text-h3 text-center my-6">
      Pokédex
    </h1>
    <!-- TODO: Afficher les Pokémon ici -->
    <v-row>
      <!--
      v-for crée une colonne pour chaque Pokémon
        * :key est obligatoire — identifiant unique pour Vue
        * cols="12" : 1 carte/ligne sur mobile (12/12 = 100%)
        * sm="6" : 2 cartes/ligne sur tablette (6/12 = 50%)
        * md="4" : 3 cartes/ligne sur écran moyen (4/12 = 33%)
        * lg="3" : 4 cartes/ligne sur grand écran (3/12 = 25%)
      -->
      <v-col
        v-for="pokemon in pokemons"
        :key="pokemon.id"
        cols="12"
        sm="6"
        md="4"
        lg="3"
      >
        <v-card>
          <!--
          Image du Pokémon
            * :src lie dynamiquement le chemin de l'image
            * height="200" fixe la hauteur
            * cover remplit l'espace en gardant les proportions
          -->
          <v-img
            :src="getImageUrl(pokemon.img)"
            :alt="pokemon.name"
            height="200"
            cover
          />
          <!-- Nom du Pokémon -->
          <v-card-title>{{ pokemon.name }}</v-card-title>
          <!-- Niveau du Pokémon -->
          <v-card-subtitle>Niveau {{ pokemon.level }}</v-card-subtitle>
        </v-card>
      </v-col>
    </v-row>
  </v-container>
</template>

<script setup>
// Vos scripts ou imports ici
import { ref, onMounted } from 'vue'
import { getImageUrl } from '@/utils/imageUrl'

// État réactif
const pokemons = ref([])
const loading = ref(true)
const error = ref(null)

// TODO : Charger les personnages depuis l'API Rick and Morty
// URL : https://rickandmortyapi.com/api/character
// La réponse contient { info: {...}, results: [...] }
onMounted(async () => {
  try {
    const response = await fetch('http://localhost:3535/pokemons')

    if (!response.ok) {
      throw new Error(`Erreur HTTP ${response.status}`)
    }

    const data = await response.json()
    pokemons.value = data

  } catch (err) {
    error.value = `Impossible de charger les animes : ${err.message}`
  } finally {
    loading.value = false
  }
  console.log('Pokemon chargés :', pokemons.value)
})
</script>
