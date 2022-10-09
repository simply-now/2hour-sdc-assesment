<template>
    <div class="py-3 px-5 has-background-sdc-grey has-border-bottom">
      <div class="container">
        <ul>
            <li v-for='item in pokemonMiniCardList'>
            {{ item }}
            </li>
        </ul>
      </div>
    </div>
  </template>
  
  <script>
  export default {
    name: 'PokemonCards',
    props: {
        pokemonCardList: {
            type: Array,
            required: true
        }
    },
    data () {
        return {
            pokemonCardDataList: ["Loading..."],
            pokemonMiniCardList: ["Loading..."]
        }
    },
    created () {
    if (this.pokemonCardList && this.pokemonCardDataList && this.pokemonMiniCardList) {
        this.pokemonCardDataList = [];
        this.pokemonMiniCardList = [];
        console.log(this.pokemonCardList)
        let TempPokemonCardList = [...this.pokemonCardList]
        console.log(TempPokemonCardList)
        TempPokemonCardList.forEach((element, index) => {
        console.log(element);
        let pokemonName = element.toLowerCase()
        let pokemonNameUrl = "https://pokeapi.co/api/v2/pokemon/" + pokemonName
        console.log("Request:"+ pokemonNameUrl)
        let requestOptions = {method: 'GET', redirect: 'follow'}
        fetch(pokemonNameUrl, requestOptions)
            .then(response => response.text())
            .then(result => {
                let data = JSON.parse(result);
                let image = data.sprites.other['official-artwork'].front_default
                let abilities = data.abilities
                let types = data.types
                let stats = data.stats
                let miniCardList = [pokemonName, image, abilities, types, stats]
                this.pokemonMiniCardList.push(miniCardList)
                console.log(miniCardList)

                this.pokemonCardDataList.push(result)
                }
                )
            .catch(error => console.log('error', error));
        });
    } else if (this.pokemonCardDataList) {
        this.pokemonCardDataList = ["Error Pokémon List Not found"]
    }
    }
  }
  </script>
  