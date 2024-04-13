<template>

    <head>
        <img src="./icons/Pokédex_logo.png" alt="">
    </head>
    <div>
        <label>INFORME A QUANTIDADE QUE DESEJA LISTAR</label><br>
        <input type="number" id="quantityPokemon" name="quantityPokemon" v-model="quantityPokemon"><br>
        <button @click="getPokemon()">Load List</button>
    </div>
    <div>
        <p></p>
    </div>
    <div>
        <label>PESQUISE O POKEMON</label><br>
        <input type="text" id="searchPokemon" name="searchPokemon"><br>
        <button @click="getSearchPokemon">Load Pokemon</button>
    </div>
    <div class="card-grid" id="pokedexlist">
        <div class="card" v-for="element in posts">
            <h2><b>{{ element.name }}</b></h2>
            <img v-bind:src="element.sprites.front_default" v-bind:alt="element.name">
        </div>
    </div>
</template>

<style>
.card-grid {
    display: grid;
    grid-template-columns: repeat(5, 1fr);
    gap: 10px;
}

.card {
    background-color: #d1c7c7;
    outline: 2px solid #ffffff;
    /* Bordas internas */
    border-radius: 8px;
    padding: 16px;
    width: 200px;
    /* Tamanho fixo para os quadros que contêm as imagens */
    background-image: linear-gradient(to bottom right, #cc0000, #f9d423);
    /* Gradiente de cor de fundo */
}

.card h2 {
    margin-bottom: 10px;
    font-size: 16px;
}

.card img {
    max-width: 100%;
    /* Imagens se ajustam ao tamanho do contêiner */
    height: auto;
    /* Mantém a proporção da imagem */
    object-fit: contain;
    /* Garante que a imagem mantenha sua proporção */
    transition: transform 0.3s ease;
    /* Transição suave da escala */
}

.card:hover img {
    transform: scale(1.1);
    /* Aumenta a escala da imagem ao passar o mouse */
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
            posts: [],
            quantityPokemon: null
        }
    },
    methods: {
        async getPokemon(pQuantity) {
            const qty = pQuantity ?? this.quantityPokemon;
            //var quantityPokemon = document.getElementById('quantityPokemon').value
            const response = await axios.get(`https://pokeapi.co/api/v2/pokemon?limit=${qty}&offset=0`);
            let _posts = response.data.results;

            for (var element of _posts) {
                var responseDetails = await axios.get(element.url);
                element.sprites = responseDetails.data.sprites;
            }

            this.posts = _posts;

            return this.posts;
        }
        ,
        async getSearchPokemon() {
            const pokemons = await this.getPokemon(1200);
            var namePokemon = document.getElementById('searchPokemon').value
            const filtered = pokemons.filter(p => p.name.includes(namePokemon));

            this.posts = filtered;
        }
    }
}

</script>

<style scoped></style>
