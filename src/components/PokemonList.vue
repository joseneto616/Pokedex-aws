<template>
    <div>
       <label class="label">INFORME A QUANTIDADE QUE DESEJA LISTAR</label><br>
        <input type="number" id="quantityPokemon" name="quantityPokemon" v-model="quantityPokemon"><br>
        <button @click="getPokemon">Carregar lista</button>
    </div>
    <div><p></p></div>
    <div>
        <label class="label">PESQUISE O POKEMON</label><br>
        <input type="text" id="searchPokemon" name="searchPokemon"><br>
        <button @click="getSearchPokemon">Carregar Pokemon</button>
    </div>
    <div class="card-grid" id="pokedexlist"></div>
</template>

<style>

button {
    margin-top: 5px;
    background-color: #F49240;
    border: none;
    border-radius: 10px;
    padding: 10px;
    
}

button:hover{
        background-color: #F9642C ;
    }

.label {
    color: black;
}

.card-grid {
  display: grid;
  grid-template-columns: repeat(5, 1fr);
  gap: 10px;
}

.card {
  background-color: #d1c7c7;
  outline: 2px solid #ffffff; /* Bordas internas */
  border-radius: 8px;
  padding: 16px;
  width: 200px; /* Tamanho fixo para os quadros que contêm as imagens */
  background-image: linear-gradient(to bottom right, #cc0000, #f9d423); /* Gradiente de cor de fundo */
}

.card h2 {
  margin-bottom: 10px;
  font-size: 16px; 
}

.card img {
  max-width: 100%; /* Imagens se ajustam ao tamanho do contêiner */
  height: auto; /* Mantém a proporção da imagem */
  object-fit: contain; /* Garante que a imagem mantenha sua proporção */
  transition: transform 0.3s ease; /* Transição suave da escala */
}

.card:hover img {
  transform: scale(1.1); /* Aumenta a escala da imagem ao passar o mouse */
}

.card p {
  margin-bottom: 10px;
}
</style>

<script>
import axios from 'axios'
export default {
    name: 'PokemonList',
    data() {
        return {
            post: [],
            quantityPokemon: null
        }
    },
    methods: {
        getPokemon() {
            // var quantityPokemon = document.getElementById('quantityPokemon').value
            axios.get('https://pokeapi.co/api/v2/pokemon?limit=' + this.quantityPokemon + '&offset=0')
                .then((response) => {
                    this.posts = response.data.results
                    var pokedexlist = document.getElementById('pokedexlist')
                    pokedexlist.innerHTML = "";
                    this.posts.forEach(element => {
                        axios.get(element.url)
                            .then((response) => {
                                pokedexlist.innerHTML += `
                                    <div class="card">
                                        <h2>${element.name}</h2>
                                        <img src="${response.data.sprites.front_default}" alt="${element.name}">
                                    </div>`;
                            })
                            .catch((error) => {
                                console.log(error)
                            })
                    });
                })
                .catch((error) => {
                    console.log(error)
                })
        },
        getSearchPokemon() {
            var namePokemon = document.getElementById('searchPokemon').value
            axios.get('https://pokeapi.co/api/v2/pokemon?limit=1000000&offset=0')
                .then((response) => {
                    this.posts = response.data.results
                    var pokedexlist = document.getElementById('pokedexlist')
                    pokedexlist.innerHTML = "";
                    this.posts.forEach(element => {
                        if (element.name.includes(namePokemon)) {
                            axios.get(element.url)
                                .then((response) => {
                                    pokedexlist.innerHTML += `
                                        <div class="card">
                                            <h2>${element.name}</h2>
                                            <img src="${response.data.sprites.front_default}" alt="${element.name}">
                                        </div>`;
                                })
                                .catch((error) => {
                                    console.log(error)
                                })
                        }
                    });
                })
                .catch((error) => {
                    console.log(error)
                })
        }
    }
}
</script>

<style scoped></style>
