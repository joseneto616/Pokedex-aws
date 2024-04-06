<template>
    <di>
        <label>Informe A Quantidade Que Deseja Listar</label>
        <input type="number" id="quantityPokemon" name="quantityPokemon"><br>
    </di>
    <div>
        <button @click="getPokemon">Load Pokemon</button>
        <h3>Poke-Dex</h3>
    </div>
    <div id="pokedexlist"></div>
</template>

<script>
    import axios from 'axios'
    export default{
        name: 'PokemonList',
        data() {
            return {
                post: [],
            }
        },
        methods: {
            getPokemon(){
                var quantityPokemon = document.getElementById('quantityPokemon').value
                axios.get('https://pokeapi.co/api/v2/pokemon?limit='+quantityPokemon+'&offset=0')
                    .then((respose) => {
                        this.posts = respose.data.results
                        var pokedexlist = document.getElementById('pokedexlist')
                        pokedexlist.innerHTML = "";
                        this.posts.forEach(element => {
                            axios.get('https://pokeapi.co/api/v2/pokemon/'+ element.name)
                                .then((respose) => {
                                    
                                    pokedexlist.innerHTML += `

                                        <div class="card-grid">
                                        <div class="card">
                                            <h2>`+element.name+`</h2>
                                            <img src="`+respose.data.sprites.front_default+`" alt="Card 1">
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
            }
        }
    }
</script>

<style scoped>
</style>