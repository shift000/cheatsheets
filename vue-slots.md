# `main.js`

```javascript
import SlotComp from './components/SlotComp.vue';
...
app.component('slot-comp', SlotComp);
app.mount('#app');
```

# App.vue
```vue
<template>
  <div id="wrapper">
    <slot-comp v-for="x in foods">
      // Durch slot werden elemente an componente übergeben
      <img v-bind:src="x.url">
      <h4>{{ x.name }}</h4>
      <p>{{ x.desc }}</p>
    </slot-comp>

    <slot-comp v-slot:bottomSlot>       // wird in bottomSlot eingefügt
      <h3>Hallo</h3>
    </slot-comp>

    <slot-comp v-slot:default>
      <template #bottomSlot>            // # ist alias für v-slot:
        <h3>Bottom</h3>                 // in einem slot können mehrere templates verwendet werden
        <p>Ich bin im bottomSlot</p>
      </template>
      <p>Ich bin im default Slot</p>
    </slot-comp>

    <slot-comp v-slot:"dataFromSlot">
      <h2>{{ dataFromSlot.lclData }}</h2>
    </slot-comp>

    <slot-comp v-slot="food">           // -> scoped slot empfängt und
      <h2>{{ food.foodName }}</h2>      // App.vue kontrolliert hier was in component
    </slot-comp>                        // angezeigt wird
  </div>
</template>
```

# SlotComp.vue
```vue
<template>
  // componente stellt gerüst dar, alles in übergabe
  // wird in <slot></slot> eingefügt
  <div>
    <slot v-bind:lclData="data">
      <h4>wird nichts übergeben wird das hier angezeigt</h4>
    </slot>

    <slot name="bottomSlot">                   // wenn kein name definiert wird in jeden slot das
      <p>Ein slot kann auch ein name haben</p> // gleiche eingefügt
    </slot>                                    // wenn kein name, dann ist dieser slot
  </div>                                       // v-slot:default

  <slot
    v-for="x in foods"                // -> scoped slot sendet daten aus object-array
    :key="x"                          // Hier stellt das template des component die struktur
    :foodName="x"                     // + daten im template bereit
    staticText="ich bleibe so"        // dieser text ist mit v-bind an x gebunden (dynamisch)
  ></slot>                            // dieser text ändert sich nicht ( statisch)
</template>

<script>
export default {
  data() {
    return {
      data: 'this is local data',
      foods: ['Apple', 'Pizza', 'Rice']
    };
  }
};
</script>

<style scoped>
div {
  ...
}
div:hover {
  ...
}
</style>
```
