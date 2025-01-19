# `#!.vue` - Aufbau (vue component)

```vue
<template>
  // html-inhalt
  <div [Directive]
    v-bind      // Erstelle Verbindung zu tag-attribute wie class, src, style
    v-if        // Erstellung von tag durch Bedingung
    v-show      // Sichtbarkeit durch Bedingung
    v-for       // Erstellung von tagliste durch Schleife
    v-on        // Erstellt Verbindung mit Event
      :click    // Event Klick
      :input    // Event Eingabe
      :keydown  // Event Tastenanschlag
    v-model     // Verbindung von input (value) mit datenwert in instance (script-variablen)
  >
    <h1>Hallo Welt</h1>
    <img src="image.svg" v-show:"isVisible">
    <button v-on:click.once="makeVisible">Mach sichtbar</button>   // Event Modifier .once lässt nur einmal nach refresh klicken
      v-on:keydown.ctrl.s                                         // Kombination von Eingaben CTRL + S
      v-on:click.right.ctrl.prevent                               // Kombination Rechtsklick mit CTRL ohne Öffnen des Rechtsklick-Menüs
  </div>

  <div id="form-input">                                           // Eingabe per Formular
    <form>
      <p>Add item</p>
      <p>Name: <input type="text" required v-model="itemName"></p>
      <p>Anzahl: <input type="number" v-model="itemNumber"></p>
      <button type="submit">Add</button>
    </form>
    
    <p>Liste</p>                                                 // Anzeige einer Liste mit v-for
    <ul>
      <li v-for="item in itemList">{{ item.name }}, {{ item.number }}</li>
    </ul>
  </div>
  // [!] Wenn in App.vue mehrere elemente mit v-for erstellt werden, muss key zur eindeutigen
  // Identifikation verwendet werden
  // <food-item v-for="x in foods" :key="x.name" :food-name="x.name" :food-desc="x.desc"/>

  <li v-bind="$attrs">{{ itemName }}</li>   // dieses element erhält den inhalt von style=".." aus aufrufendem in App.vue
...
</template>

KEYS: .enter .tab .delete .esc .space .up .down .left .right  
CHAR: .a .b .c .d .e .f .g .h .i .j .k .l .m .n .o .p .q .r .s .t .u .v .w .x .y .z  
SYS : .alt .ctrl .shift .meta  
-> Alles ein Kombination möglich

<script>
export default {
  props: ['name', 'visible'],   // variablen für componente beim create in App.vue
                                // wie <componenteA varA="Hallo" varB="Bye"/>
  // oder
  props: {                      // übergabe von objekten
    visible: {			
      type: boolean,            // Datentyp
      required: true,           // Muss ausgefüllt werden?
      default: false,           // Standard-Platzhalter
      validator: function(value) {  // Funktion zur Überprüfung korrekter Eingabe
        if (value) {            // Kann bei type=String bspw. Länge vergleichen value.length > 20
          return true;
        } else {
          return false;
        }
      }
    }
  }, // Ende des props -> Nur jeweils einer der oben kann benutzt werden

  data() {
    return {
      isVisible: this.visible,
      itemName: null,
      itemNumber: null,
      itemList: [
        { name: "Tomato", number: 5 }
      ]
    }
  },

  methods: {
    makeVisible() {
      this.isVisible = !this.isVisible;
    },
    
    addItem() {
      let item = {
        name: this.itemName,
        number: this.itemNumber
      }
      this.itemList.push(item);
      this.itemName = null;
      this.itemNumber = null;
    },
  
    emitToApp() { // in App.vue wird component angelegt mit <food-item v-for="x in foods"
      this.$emit('toggle-emit', this.name); // .. @emit-to-app="receiveEmit"/>
    }                                       // receiveEmit(name) ist methode in App.vue mit übergabe von parameter
  }
};
</script>

<style scoped>  // scoped = style wird nur in componente aktiv statt global


</style>
