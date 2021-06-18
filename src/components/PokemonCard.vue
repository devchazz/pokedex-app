<template>
  <div>
    <div class="card">
      <div class="card-top">
        <div>
          <img class="pokemon-sprite" :src="currentImg">
        </div>
        <div class="card-content">
          <div class="card-text">
            <h2>{{name.toUpperCase()}}</h2>
          </div>
          <div class="card-button">
            <button @click="changeSprite">Rotate sprite</button>
          </div>
        </div>
      </div>
      <div class="card-bottom">
        <img :src="cuteSprite">
        <p>Type: {{pokemon.type.toUpperCase()}}</p>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios'

export default {
  created: function(){
    axios.get(this.url).then(res=>{
      this.pokemon.type = res.data.types[0].type.name
      this.pokemon.front = res.data.sprites.front_default
      this.pokemon.back = res.data.sprites.back_default
      this.cuteSprite = `https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/versions/generation-vii/icons/${this.pokeId+1}.png`
      this.currentImg = this.pokemon.front
      //this.name = this.name.charAt(0).toUpperCase() + this.name.slice(1);
    })
  },
  data(){
    return{
      currentImg: '',
      cuteSprite: '',
      pokemon: {
        type: '',
        front: '',
        back: '',
      }
    }
  },  
  props: {
    name:String,
    url:String,
    pokeId:Number
  },
  methods: {
    changeSprite(){
      if(this.currentImg == this.pokemon.front){
        this.currentImg = this.pokemon.back
      }else{
        this.currentImg = this.pokemon.front
      }
    },
  }
}
</script>

<style>
  .card{
    width: 480px;
    border: 2px solid black;
    background-color: white;
    border-radius: 10px;
    margin: 5px 10px;
  }
  .card-top{
    display: flex;
    justify-content: space-between;
    align-items: center;
    border-bottom: 2px solid black;
    border-radius: 3px;
    padding: 0 10px;
  }
  .card-content{
    flex: 3;
    padding: 0 15px;
    display: flex;
    justify-content: space-between;
    align-items: center;
  }
  .card-text{
    flex: 2;
    text-align: start;
  }
  .card-button button{
    padding: 10px;
  }
  .card-bottom{
    padding: 10px 15px;
    display: flex;
    justify-content: space-between;
    align-items: center;
  }
  @media screen and (max-width: 600px) {
    .card{
      width: 270px;
    }
    .card-top, .card-content{
      flex-direction: column;
      justify-content: space-around;
    }
    .card-content button{
      margin: 10px 0;
    }
  }
</style>