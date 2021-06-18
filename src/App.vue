<template>
  <div id="app">
    <div class="h-100">
      <Header/>
      <div class="search-box">
        <div class="search-box-content">
          <input @keyup="searchByName" id="input-search" placeholder="Search" type="text">
          <div></div>
        </div>
      </div>
      <div class="container-cards">
        <div v-for="(e,i) in pokemons" :key="e.url">
          <PokemonCard :nemOlhe=i :pokeId="e.id" :name="e.name" :url="e.url"/>
        </div>
      </div>
    </div>
    <Footer/>
  </div>
</template>

<script>
import axios from 'axios'
import Header from './components/Header.vue'
import PokemonCard from './components/PokemonCard'
import Footer from './components/Footer.vue'

export default {
  name: "App",
  data(){
    return{
      allPokemons: [],
      pokemons: [],
      filter: ''
    }
  },
  components:{
    Header,
    PokemonCard,
    Footer
  },
  methods:{
    searchByName(){
      let filter = document.getElementById('input-search').value.toUpperCase()
      if(filter){
        this.pokemons = this.allPokemons.filter(e => {
          if(e.name.toUpperCase().indexOf(filter) >= 0){
            return e
          } 
        })
      }else{
        this.pokemons = this.allPokemons
      }
    }
  },
  created: function(){
    let i = 0
    axios.get("https://pokeapi.co/api/v2/pokemon?limit=151&offset=0").then(res=>{
      this.allPokemons = []
      for(let e of res.data.results){
        this.allPokemons.push({
          id: i,
          name: e.name,
          url: e.url
        })
        i++
      }
      this.pokemons = this.allPokemons
    })
  }
};
</script>

<style>
*{
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  background-color: rgb(223, 218, 218); 
}
.h-100{
  min-height: 92vh;
}
.search-box{
  position: sticky;
  top: 0;
  background-color: #222;
  border-top: 3px solid red;
  border-bottom: 3px solid red;
}
.search-box-content{
  margin: auto;
  max-width: 1000px;
  display: flex;
  justify-content: space-between;
  padding: 10px 0;
}
.search-box-content input{
  color: white;
  font-size: 1.2em;
  margin: 0 10px;
  padding: 3px;
  background-color: #222;
  border: white 3px solid;
  border-radius: 5px;
}
.search-box-content input:hover{
  border: rgb(255, 0, 0) 3px solid;
}
.search-box-content input:focus{
  outline: none;
  border: rgb(255, 0, 0) 3px solid;
}
.search-box-content button{
  padding: 3px;
  font-size: 1.2em;
}
.container-cards{
  margin: auto;
  text-align: center;
  max-width: 1000px;
  display: flex;
  flex-direction: row;
  flex-wrap: wrap;
  justify-content: center;
}
</style>
