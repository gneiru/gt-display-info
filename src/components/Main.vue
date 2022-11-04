<script>
import axios from 'axios';
let baseUrl = import.meta.env.VITE_HERO_API;

export default {
  el: '#aw',
  data () {
    return {
      infos: null,
      loading: true,
      errored: false
    }
  }, 
  mounted () {
    axios
      .get(baseUrl)
      .then(response => {
        this.infos = Object.keys(response.data).sort().reduce(
          (obj, key) => { 
            obj[key] = response.data[key]; 
            return obj;
          }, 
          {}
        );
      })
      .catch(error => {
        console.log(error)
        this.errored = true
      })
      .finally(() => this.loading = false)
  }
}
</script>

<template>
  <div id="aw">
    <h1>Guardian Tales</h1>
  
    <section v-if="errored">
      <p>We're sorry, we're not able to retrieve this information at the moment, please try back later</p>
    </section>
  
    <section v-else>
      <div v-if="loading">Loading...</div>
  
      <div class="grid"
        v-else
      >
        <div class="card" v-if="typeof infos == 'object'" v-for="info in infos">
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
  
    </section>
  </div>
</template>

<style scoped>
.grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(350px, 1fr));
  grid-gap: 20px;
  align-items: stretch;
}
.card {
  background-color: #212121;
  box-shadow: 15px 15px 30px #191919,
               -15px -15px 30px #292929;
  transition: border-radius cubic-bezier(0.075, 0.82, 0.165, 1) 1s,
               transform cubic-bezier(0.075, 0.82, 0.165, 1) 1s;
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

</style>
