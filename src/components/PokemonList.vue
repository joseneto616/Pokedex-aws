<template>
    <div>
        <label>Informe A Quantidade Que Deseja Listar</label><br>
        <input type="number" id="quantityPokemon" name="quantityPokemon"><br>
        <button @click="getPokemon">Load List</button>
    </div>
    <div><p></p></div>
    <div>
        <label>Pesquise Pokemon</label><br>
        <input type="text" id="searchPokemon" name="searchPokemon"><br>
        <button @click="getSearchPokemon">Load Pokemon</button>
    </div>
    <div id="pokedexlist"></div>
</template>

<script>
import axios from 'axios'
export default {
    name: 'PokemonList',
    data() {
        return {
            post: [],
        }
    },
    methods: {
        getPokemon() {
            var quantityPokemon = document.getElementById('quantityPokemon').value
            axios.get('https://pokeapi.co/api/v2/pokemon?limit=' + quantityPokemon + '&offset=0')
                .then((respose) => {
                    this.posts = respose.data.results
                    var pokedexlist = document.getElementById('pokedexlist')
                    pokedexlist.innerHTML = "";
                    this.posts.forEach(element => {
                        axios.get('https://pokeapi.co/api/v2/pokemon/' + element.name)
                            .then((respose) => {

                                pokedexlist.innerHTML += `

                                        <div class="card-grid">
                                        <div class="card">
                                            <h2>`+ element.name + `</h2>
                                            <img src="`+ respose.data.sprites.front_default + `" alt="Card 1">
                                        </div>
                                        `
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
                .then((respose) => {
                    this.posts = respose.data.results
                    var pokedexlist = document.getElementById('pokedexlist')
                    pokedexlist.innerHTML = "";
                    this.posts.forEach(element => {
                        if (element.name.includes(namePokemon)) {
                            axios.get('https://pokeapi.co/api/v2/pokemon/' + element.name)
                                .then((respose) => {
                                    pokedexlist.innerHTML += `

                                        <div class="card-grid">
                                        <div class="card">
                                            <h2>`+ element.name + `</h2>
                                            <img src="`+ respose.data.sprites.front_default + `" alt="Card 1">
                                        </div>
                                        `
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