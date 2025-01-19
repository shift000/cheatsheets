
```javascript
import { createApp } from 'vue'					// 

import { createRouter, createWebHistory } from 'vue-router'	// Routing durch vue-router, ersetzt app.component

import App from './App.vue'					//
	
import CompFood from './components/CompFood.vue'		// componente laden

// Standard
const app = createApp(App)					// App erstellen
app.component('comp-food', CompFood)				// component hinzufügen
// Ende

// Mit Router
const router = createRouter({					// Router für Komponenten
 history: createWebHistory(),
 routes: [
  { path: '/site1', component: CompFood },
  { path: '/site2', component: CompFood }
 ]
});

const app = createApp(App)
app.use(router)
/// End

app.mount('#app')						// Alles packen und mounten
```
