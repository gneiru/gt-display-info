<script>
import axios from 'axios';
let baseUrl = import.meta.env.VITE_HERO_API;

export default {
  el: '#aw',
  data () {
    return {
      info: null,
      infos: null,
      loading: true,
      errored: false,
      search: "",
      one: false
    }
  }, 
  mounted () {
    axios
      .get(baseUrl)
      .then(response => {
        if (this.search != ""){
          this.info = response.data[this.search]
          this.one = true;
        }else{
          this.infos = Object.keys(response.data).sort().reduce(
          (obj, key) => { 
            obj[key] = response.data[key]; 
            return obj;
          }, 
          {}
        );
          this.one = false;
        }
      })
      .catch(error => {
        console.log(error)
        this.errored = true
      })
      .finally(() => this.loading = false)
  },
  methods: {
    changeOne(){
      axios
      .get(baseUrl)
      .then(response => {
          this.info = response.data[this.search]
          this.one = true
      })
      .catch(error => {
        console.log(error)
        this.errored = true
      })
      .finally(() => this.loading = false);
    },
    fetchAll(){
      axios
      .get(baseUrl)
      .then(response => {
          this.info = response.data
          this.one = false
      })
      .catch(error => {
        console.log(error)
        this.errored = true
      })
      .finally(() => this.loading = false);
    }
  },
}
</script>

<template>
  <div id="aw">
    <h1>Guardian Tales</h1>
    <div class="search">
      <input type="text" v-model="search" placeholder="Xellos" @keyup.enter="changeOne">
      <button @click="changeOne">Search</button>
      <button @click="fetchAll">Reset</button>
    </div>
    <section v-if="errored">
      <p>We're sorry, we're not able to retrieve this information at the moment, please try back later</p>
    </section>
  
    <section v-else>
      <div v-if="loading">Loading...</div>
      
      <div class="div" v-if="one">
        <div class="card" style="max-width: 400px;">
          <img :src="`https://raw.githubusercontent.com/gneiru/gt-assets/main/icon/heroes/${info.key}.png`">
          <h3>{{ info.name }}</h3>
          <label class="label">Element: </label>{{ info.element }}<br>
          <label class="label">Class: </label>{{ info.class }}<br>
          <label class="label">Exclusive Weapon: </label>{{ info.exw }}<br>
          <label class="label">Compatible Equipment: </label>{{ info.equips }}<br>
          <label class="label">Party Buff: </label>{{ info.party_buff }}<br>
          <label class="label">Rarity: </label>{{ info.rarity }}<br><br>
          <label class="label">Normal Attack: </label>{{ info.normal_title }}<br>{{ info.normal_desc }}<br><br>
          <label class="label">Chain Skill: </label>{{ info.chain_title }}<br>{{ info.chain_desc }}<br><br>
          <label class="label">Special Skill: </label>{{ info.special_title }}<br>{{ info.special_desc }}<br>
        </div>
      </div>
      
      <div
        v-else  class="grid"
      >
        <div v-for="info in infos" v-if="one == false">
          <div class="card">
            <img :src="`https://raw.githubusercontent.com/gneiru/gt-assets/main/icon/heroes/${info.key}.png`">
            <h3>{{ info.name }}</h3>
            <label class="label">Element: </label>{{ info.element }}<br>
            <label class="label">Class: </label>{{ info.class }}<br>
            <label class="label">Exclusive Weapon: </label>{{ info.exw }}<br>
            <label class="label">Compatible Equipment: </label>{{ info.equips }}<br>
            <label class="label">Party Buff: </label>{{ info.party_buff }}<br>
            <label class="label">Rarity: </label>{{ info.rarity }}<br><br>
            <label class="label">Normal Attack: </label>{{ info.normal_title }}<br>{{ info.normal_desc }}<br><br>
            <label class="label">Chain Skill: </label>{{ info.chain_title }}<br>{{ info.chain_desc }}<br><br>
            <label class="label">Special Skill: </label>{{ info.special_title }}<br>{{ info.special_desc }}<br>
          </div>
        </div>
        
        
        
      </div>
  
    </section>
  </div>
</template>

<style scoped>
.single-card {
  box-shadow: rgba(0, 0, 0, 0.25) 0px 14px 28px, rgba(0, 0, 0, 0.22) 0px 10px 10px;
}
.grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(350px, 1fr));
  grid-gap: 20px;
  align-items: stretch;
}
.card {
  display: block;
  margin-left: auto;
  margin-right: auto;
  background-color: #212121;
  box-shadow: rgba(0, 0, 0, 0.25) 0px 14px 28px, rgba(0, 0, 0, 0.22) 0px 10px 10px;
  text-align: left;
 }
 .card img {
  position: relative;
  float:right;
  border-radius: 40px;
  max-width: 100px;

 }
 .card:hover {
  border-bottom-right-radius: 50px;
  border-top-left-radius: 50px;
  transform: scale(1.05);
 }
.label {
  font-weight: bold;
}
.search{
  padding: 4px;
  margin-bottom: 20px;
}
.search button{
  background-color: #212121;
  outline: solid #212121;
  color: white;
  height: 40px;
  border-radius: 10px;
  margin: 4px;
}
.search button:hover{
  outline-style: solid;
  outline-color: white;
}
.search input {
  height: 40px;
  line-height: 28px;
  padding: 0 1rem;
  border: 2px solid transparent;
  border-radius: 8px;
  outline: none;
  background-color: #f3f3f4;
  color: #0d0c22;
  transition: .3s ease;
 }

</style>
