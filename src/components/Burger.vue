<template>

  <div id="burger" style= "float:left;">
    <h2>{{ burger.name }}</h2> 
    <img v-bind:src="burger.img" height="190" width="240" alt="No image"
           title="alt burger" />
<dd>
      <ul>
        <li>Antal kalorier: {{burger.kCal}} </li>
        <li v-if="burger.gluten"><span class="gluten" v-if="burger.gluten">Innehåller gluten</span></li>
        <li v-if="burger.lactose"><span class="lactose" >Innehåller laktos</span></li>
      </ul>
    </dd>
   
 {{amountOrdered}}
 <p id="Antal">Antal: <button v-on:click= "subtrahera">-</button>
 <button v-on:click= "addera">+</button></p>
  </div>
 
</template>

<script>
export default {
  name: 'Burger',
  props: {
    burger: Object
  },

  data: function () {
  return {
    amountOrdered: 0,
  }
},

methods: {
   subtrahera: function () {
     if (this.amountOrdered > 0) {
     this.amountOrdered --
     this.$emit('orderedBurger', { name:   this.burger.name, 
                                amount: this.amountOrdered 
                              }
  );
     
}},
   addera: function () {
     this.amountOrdered ++
     this.$emit('orderedBurger', { name:   this.burger.name, 
                                amount: this.amountOrdered 
                              }
  );

}
},
}



</script>


<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  /*empty*/
body {
   font-family: "Times New Roman";
   

}
.Introbild {
   position: absolute;
   color:cornflowerblue;
}
.Specs {
   font-weight: bold;
}
#burgerselection {
   background-color: black;
   color: white;
   border: 5px ridge #ff9900;
}
#burger {
   margin-right: 50px;
   margin-bottom: 10;
}
button:hover {
   color:cornflowerblue;
   cursor: pointer;
   background-color: rgb(20, 248, 218);
}
section {
   margin-top: 10px;
   margin-left: 10px;
   margin-bottom: 10px;
   margin-right: 50px;
   padding: 1px 23px 4px;
   
 }
 #Customerinfo {
   
  
   border: 5px dotted #0a0a0a;
}

  #map {
    width: 300px;
    height: 300px;
    background-color: red;
  }
</style>
