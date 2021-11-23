<template>
 <section id = "burgerselection">
    <header id="header">
    <img src="https://warsawinsider.pl/wp-content/uploads/2015/08/12628455_1055887724472069_5561130644842687375_o-750x350.jpg" alt="alt text"/>
    <h1>Burger Mania</h1>
  </header>
 
  <h1>Välj din burgare</h1>
  <div style = "margin-left: 100px;" >
    <Burger v-for="burger in burgers"
            v-bind:burger="burger" 
            v-bind:key="burger.name"
            v-on:orderedBurger="addToOrder($event)"/>
  </div>
   
    <section id = "Customerinfo" style="clear: left;">
        <h2>Information om dig</h2>
        <section id="contact">
            
            <form>
                <p>
            
                    <label for="firstname">Förnamn och efternamn</label><br>
                    <input type="text" id="name" v-model= "name" required="required" placeholder="ex sir Patrick of Wales">
                </p>
            
                
                <p>
                    <label for="Emailadress">Emailadress</label><br>
                    <input type="email" id="email" v-model= "email" required="required" placeholder="E-mail address">
                </p>
                <p>
                    <label for="paymethod">Betalningsmetod</label><br>
                    <select id="paymentmethod" v-model= "paymethod">
                        <option>Swish</option>
                        <option>Kortbetalning</option>
                        <option>Natura</option>
                        <option>Faktura</option>
                    
                    </select>
                 </p>
                 <p>Kön:</p>

                <div>
                <input type="radio" id="man" v-model="sex" value="Man"
                        checked>
                <label for="Man">Man</label>
                </div>

                <div>
                <input type="radio" id="kvinna" v-model="sex" value="Kvinna">
                <label for="Kvinna">Kvinna</label>
                </div>

                <div>
                <input type="radio" id= "oklart" v-model="sex" value="Vill inte välja">
                <label for="Vill inte välja">Vill inte välja</label>
                </div>
            </form>
           <br>
      <h3>Välj din adress på kartan nedan</h3>     
      <div id="maphandler" >
        <div id="map" v-on:click= "setLocation" >
          <div id="dot" >

          <div v-bind:style="{ left: location.x + 'px', top: location.y + 'px'}">
              T
            </div>
  </div>
      </div>
      </div>
        </section>
    </section>
        <div>
            <br>
            <br>
        <button v-on:click = submitOrder type="submit">
            <img src="https://www.freeiconspng.com/uploads/submit-button-png-25.png" style="width: 18px;">
            Beställ
          </button>
        </div>
       
       <hr>
       </section>
   <footer>Den som försöker sno våra recept åker på däng &copy; copyright 

   </footer>
</template>

<script>
import Burger from "../components/Burger.vue"
import io from "socket.io-client"
import menu from "../assets/menu.json"

const socket = io();

/* function MenuItem(name,kcal,glut,lact,vegan,img){
  this.name = name; //Burgarnamn
  this.calories = kcal; //Kalorimängd
  this.gluten_free = glut; //Allergener
  this.lactose_free = lact; //Allergener
  this.img = img; //Bild
} */
const burgers = menu /* [new MenuItem("Epic Burger", 900, true, true, false, "pic"),
                new MenuItem("Hot Madness",1000, false, true, false, "pic"),
                new MenuItem("Mean and green", 1200, true, true, true, "pic") ]  */
export default {
  name: 'Home',
  components: {
    Burger
  },

  data: function () {
    return {
      burgers: burgers,
      name: "",
      email: "",
      sex: "",
      paymethod: "Swish",
      adress_number: "",
      adress: "",
      orderedBurgers: {},
      location: { x: 0,
        y: 0

}
    }
  },
  methods: {
    getOrderNumber: function () {
      return Math.floor(Math.random()*100000);
    },
    setLocation: function (event) {
      var offset = {x: event.currentTarget.getBoundingClientRect().left,
                    y: event.currentTarget.getBoundingClientRect().top};
      this.location.x = event.clientX - 10 - offset.x;
      this.location.y = event.clientY - 10 - offset.y;
      
    },
  
   submitOrder: function () {
             
                    /*   console.log(this.name),
                      console.log(this.email),
                      console.log(this.paymethod),
                      console.log(this.sex),
                      console.log(this.adress),
                      console.log(this.houseNumber)
                      console.log(this.orderedBurgers) */

     socket.emit("addOrder", { orderId: this.getOrderNumber(),
                                details: { x: this.location.x, y: this.location.y,
                                           namn: this.namn, email: this.email, sex: this.sex, paymethod: this.paymethod},
                                orderItems: [this.orderedBurgers]
                              }
                 );       
          
          },
  addToOrder: function (event) {
    this.orderedBurgers[event.name] = event.amount;
  },
 },
  
} 
    
</script>

<style>

body {
   font-family: "Times New Roman";
   

}


#header h1 {
  position: absolute;
  margin-top: -300px;
  padding-left: 300px;
  font-size: 60pt;
  text-align: center;
  color: purple;
}
#dot {
  position: relative;
  margin: 0;
  padding: 0;
  background: url(/img/polacks.jpg);
  background-repeat: no-repeat;
  width:1920px;
  height: 1078px;
  cursor: crosshair;
}

#dot div {
  position: absolute;
  background: black;
  color: white;
  border-radius: 10px;
  width:20px;
  height:20px;
  text-align: center;
}
#header{
  margin-left: 50px;
  margin-right: 50px;
  overflow: hidden;
  height: 370px;
   
}
#header img {
  opacity: 0.8;
  width: 100%;
  height: auto;
}
.Specs {
   font-weight: bold;
}
#burgerselection {
   background-color: black;
   color: white;
   border: 5px ridge #ff9900;
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
   
  
   border: 5px dotted white;
}

  #map {
    width: 10290px;
    height: 1078px;
    background: url("/img/polacks.jpg");
    color: purple;

  }
  #maphandler {
    width: 500px;
    height: 400px;
    overflow: scroll;
  }
</style>
