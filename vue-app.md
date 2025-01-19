
```vue
<template>
 <div>
  <h3> Local </h3>
  <comp-one /> <br>						// lade comp-one aus script  (local)
  <comp-two />							// lade comp-two aus main.js (global)
 </div>

 <button @click="activeComp = 'comp-one'">CompOne</button>	// Componente One dynamisch laden 
 <button @click="counter++">Add</button>			// counter wird pro klick um eins erhöht
 <button @click="toggleValue != toggleValue">Switch</button>
 <KeepAlive include="CompOne">					// wenn KeepAlive angegeben, wird komponentenzustand beibehalten
  <component :is="activeComp"></component>			 // dynamisches laden einer componente durch
 </KeepAlive>							// ohne wird sie entfernt und neu geladen

 <div>
  <button @click="fetchData">Fetch</button>
  <p v-if="data">{{ data }}</p>					// <p> kann für text verwendet werden
  <pre v-if="data">{{ data }}</pre>				// <pre> für json
 </div>

 <router-link to="/site1">Seite 1</router-link>			// Routing durch 
 <router-link to="/site2">Seite 2</router-link>
 <div>
  <router-view></router-view>					// Anzeige des gerouteten
 </div>

</template>							// return von funktion

								// durch include wird nur angegeben komponente gesichert
								// durch exclude wird alles andere auser angegeben gesichert
								// mehreres kann angegeben werden include="compA,compB"
								// durch :max="2" werden nur letzte 2 besuchte komponenten gesichert

<script>
 import CompOne from './components/CompOne.vue';		// Componente lokal laden

 import axios from 'axios'					// mit axios können http requests erstellt werden

 export default {						// in aktueller .vue verfügbar machen
  data() {
   return {
    toggleValue: true
   }
  },

  components: {							//
   'comp-one': CompOne						// lade CompOne in comp-one
  },

  computed: {
   activeComp() {						// Echtzeit wechseln einer komponente
    if(this.toggleValue) {
     return 'comp-one'
    } else {
     return 'comp-two'
    }
   }
  },

  methods: {
   async fetchData() {							// asynchrone promise basierte anfragen (hier GET) müssen
    const response = await fetch("file.txt");				// auf response warten (await)
    const response = await fetch("text.json");				// json empfangen
    const response = await fetch("https://psyki.de/api/v2/users");	// daten von api empfangen (json)
    this.data = await response.text();					// da text() auch promise basiert ist muss mit await gewartet werden
    this.data = await response.json();					// json decodieren

    this.data = await axios.get("https://psyki.de/api/v2/users");
   }
  },

  provide() {								// Provide Daten für injection in componente
   return {
    foods: this.foods
   }
  }
 }
</script>
```
<style>
 ...
</style>
