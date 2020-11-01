<template>
  <div id="app">
    <h1 class="ribbon">Нова пошта</h1>
   <br>
      <a>Введіть місто</a>
   <br>
   <input type="text" v-model="city">
   <br>
      <a>{{city}}</a>
   <br>
   <button @click="q">Пошук за містом</button>
   <br>
      <a>{{cRef}}</a>
   <br>
   <button @click="otdel">Відділи</button>
   <br>

   <table v-for="el in otd" v-bind:key="el.Description">
    <td>{{el.Description}}</td>
   </table>
  </div>
</template>

<script>

import axios from 'axios'

export default {
  name: 'App',
data(){
    return {
      cities:[],
      ci:{},
      city:'',
      otd:[],
      cRef:'',
    }
  },
   mounted:function(){
    axios.post("https://api.novaposhta.ua/v2.0/json/",{
      "modelName":"AddressGeneral",
      "calledMethod":"getCities",
      "methodProperties":{
        "Warehouse":"1",
      },
      "apiKey":"9a557481f95094531372a9d1b55222c8"
    }).then((response) => {
      console.log(response.data.data);
      this.cities = response.data.data;
    })
  },
  methods:{
    q:function(){
      for (var i = 0; i < this.cities.length; i++){
      if (this.cities[i].Description == this.city){
     this.ci = this.cities[i];
     this.cRef = this.ci.Ref;
      }
    }
    },
    otdel:function(){
      axios.post("https://api.novaposhta.ua/v2.0/json/",{
    "modelName": "AddressGeneral",
    "calledMethod": "getWarehouses",
    "methodProperties": {
    "CityRef": this.cRef
    },
    "apiKey": "9a557481f95094531372a9d1b55222c8"
    }).then((response) =>{
    console.log(response.data.data);
    this.otd = response.data.data;
    })
  }
 }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: left;
  color: #2c3e50;
  margin-top: 60px;
}
h1.ribbon { 
font-size: 16px !important; 
position: relative; 
background: #ff0000; 
color: #fff; 
text-align: center; 
padding: 1em 2em;
margin: 0 0 3em;
} 
h1.ribbon:before, h1.ribbon:after { 
content: ""; 
position: absolute; 
display: block; 
bottom: -1em; 
border: 1.5em solid #bd0404; 
z-index: -1; 
} 
h1.ribbon:before { 
left: -2em; 
border-right-width: 1.5em; 
border-left-color: transparent; 
} 
h1.ribbon:after { 
right: -2em; 
border-left-width: 1.5em; 
border-right-color: transparent; 
} 
h1.ribbon .ribbon-content:before, h1.ribbon .ribbon-content:after { 
content: ""; 
position: absolute; 
display: block; 
border-style: solid; 
border-color: #bd0404 transparent transparent transparent; 
bottom: -1em; 
} 
h1.ribbon .ribbon-content:before { 
left: 0; 
border-width: 1em 0 0 1em; 
} 
h1.ribbon .ribbon-content:after { 
right: 0; 
border-width: 1em 1em 0 0; 
}

td{
  background: rgba(206, 40, 40, 0.26);
  text-align: center;
  top: 10px;
}
</style>
