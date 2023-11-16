<template>
    <header>
         <img src= "https://static1.srcdn.com/wordpress/wp-content/uploads/2019/06/The-Shire.jpg" id="Headerimg">
        <h1>Welcome to Second breakfast</h1>
    </header>
        <main>
            <section id="burger">
                <div class="Burgerselection">
                    <div class="Titel-block">
                        <h2>The best food in Middle Earth</h2>
                    </div>
                    <Burger v-for="burger in burgers" 
                                  v-bind:burger="burger" 
                                  v-bind:key="burger.name" v-on:orderedBurger="addToOrder($event)"
                                  v-on:deletedBurger="deleteFromOrder($event)"/>
                </div>
            </section>
            <section class="contact">
                <div class="block-A">
                    <form>
                        <h2>Customer Information</h2>
                        <p>Here you need to enter your contact information for the delivery.</p>
                        <div>
                            <label for="firstname">Full Name</label><br>
                            <input v-model="name" id="firstname" type="text" required="required" placeholder="First- and Last name"/>
                            <p>Your name: {{ name }}</p>
                        </div>
                        
                        <div>
                            <label for="email">E-mail</label><br>
                            <input v-model="email" id="email" type="text" required="required" placeholder="E-mail address"/>
                            <p>Your email: {{ email }}</p>
                        </div>
                        <div>
                            <label for="payment">Payment Method</label><br>
                            <select id="payment" v-model="selected">
                                <option>Silver farthing</option>
                                <option>Silver penny</option>
                                <option>Brassling</option>
                                <option>Copperlings</option>
                            </select>
                            <p>Selected payment method: {{ selected }}</p>
                        </div>
                        <div>
                            <input v-model="radio" name="choices" value="Hobbit" type="radio">
                            <label for="Hobbit">Hobbit</label>

                            <input v-model="radio" name="choices" value="Dwarf" type="radio">
                            <label for="Dwarf">Dwarf</label>

                            <input v-model="radio" name="choices" value="Elf" type="radio"> 
                            <label for="Elf">Elf </label>
                            <p>Selected: {{ radio }}</p>
                        </div>
                        <button v-on:click="handleClick" id="ringbutton">
                            <img src="https://upload.wikimedia.org/wikipedia/commons/d/d4/One_Ring_Blender_Render.png" id="ring">
                            Take the ring to Mordor!
                        </button>
                    </form>
                </div>
                <div class="block-B">
                    <div v-on:click="addOrder" id="map">
                        <div v-on:click="updatelocation" id="dot">T</div>T
                    </div>
                </div>
            </section>
        </main>
        <hr>
        <footer>
            &copy; 2023 The Golden Dragon Inc.
        </footer>
</template>

<script>
import Burger from '../components/OneBurger.vue'
import io from 'socket.io-client'
import menu from '../assets/menu.json'

const socket = io();

export default {

  name: 'HomeView',
  components: {
    Burger
  },

  data: function () {
    return {
      burgers: menu,
      orderedBurgers: {},
      name: '',
      email: '',
      street: '',
      number: null,
      selected: 'Silver farthing',
      radio: '',
      location: { x: 0,
                  y: 0}
    }
  },

  methods: {
    getOrderNumber: function () {
      return Math.floor(Math.random()*100000);
    },
    addOrder: function (event) {
      var offset = {x: event.currentTarget.getBoundingClientRect().left,
                    y: event.currentTarget.getBoundingClientRect().top};
      socket.emit("addOrder", { orderId: this.getOrderNumber(),
                                details: { x: event.clientX - 10 - offset.x,
                                           y: event.clientY - 10 - offset.y },
                                orderItems: ["Beans", "Curry"]
                              }
                 );
        location.x = event.clientX - 10 - offset.x;
        location.y = event.clientY - 10 - offset.y;
        this.moveDot();
    },

    handleClick: function () {
      const formData = {
        name: this.name,
        email: this.email,
        street: this.street,
        number: this.number,
        selected: this.selected,
        radio: this.radio
      };
      console.log('Form Data:', formData);
    },

    moveDot() {
        const dot = document.getElementById('dot');
        
        dot.style.left = location.x + 'px';
        dot.style.top = location.y + 'px';
      },

    addToOrder(order) {
        this.orderedBurgers[order.name] = order.amount;
        console.log(this.orderedBurgers);
    },

    deleteFromOrder(order) {
        if (this.orderedBurgers[order.name] > 1) {
          this.orderedBurgers[order.name] -= 1;
        } 
        else {
          delete this.orderedBurgers[order.name];
        }
        console.log(this.orderedBurgers);
    },
  }
}

</script>

<style>
  @import url('https://fonts.cdnfonts.com/css/tolkien');

  body {
      font-family: 'Tolkien', sans-serif;
      font-size: 1em;
      background-color:seagreen;
      margin: 0em;
  }

  header {
      height: 40vh;
      overflow: hidden;
      position: relative;
      font-size: x-large;
      font-weight: bolder;
      ;
      color:peru;
  }

  h1 {
      position: absolute;
      top: 50%;
      left: 50%;
      transform: translate(-50%, -50%);
  }

  #Headerimg {
      opacity: 0.60;
      width: 100%;
  }

  .contact {
      display: grid;
      gap: 1rem;
      background-color: seagreen;
      color: black;
      margin: 5vh;
      padding: 2vh;
      border: 0.5vh double peru;
      grid-template-columns: 1fr 2fr;
      grid-template-rows: 600px;
  }

  #burger {
      margin: 5vh;
      padding: 2vh;
      border: 0.5vh double peru;
      text-align: center;
  }

  button {
      margin: 1vh 2vh;
      padding: 1vh 2vh;
      border: solid black;
      cursor: pointer;
      transition: background-color 0.3s;
      text-align: center;
      border-radius: 5px;
  }

  button:hover {
      background-color: peru;
  }

  #ringbutton{
      display: flex;
      align-items: center;
  }

  #ring{
      width: 5vh;
      height: 5vh;
  }

  .Burgerselection{
      display: grid;
      gap: 1rem;
      grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
      background-color:lightsteelblue;
      color:azure;
  }

  .Titel-block{
      grid-column: 1 / span 3;
      grid-row: 1;
  }

  .block-B {
    grid-column: 2;
    grid-row: 1;
    overflow: scroll;
  }

  #map{
    background: url("../../public/img/polacks.jpg");
    width: 1920px;
    height: 1076px;
    position: relative;
    margin: 0;
    padding: 0;
    background-repeat: no-repeat;
    cursor: crosshair;
  }

  .block-A {
    grid-column: 1;
    grid-row: 1;
  }
  
  #map div {
    position: absolute;
    background: black;
    color: white;
    border-radius: 10px;
    width:20px;
    height:20px;
    text-align: center;
  }

</style>

// function MenuItem(name, url, lactose, gluten, ingredients) {
  //   this.name = name;
  //   this.url = url;
  //   this.lactose = lactose;
  //   this.gluten = gluten;
  //   this.ingredients = ingredients;
  // }
  
  // let Burgers = [ { name: "The one ring", 
  //                   url: "img/depositphotos_562899746-stock-photo-single-fried-onion-ring-_thumbnail.png", 
  //                   lactose: true, 
  //                   gluten: true, 
  //                   ingredients: "Onion, Power"},
  //                 { name: "Po-tay-toes", 
  //                   url: "img/fries-fotor-bg-remover-2023110815947.png", 
  //                   lactose: false, 
  //                   gluten: false,
  //                   ingredients: "Boil'em, Mash'em"}, 
  //                 { name: "Lembas bread", 
  //                   url: "img/dsc_2986-fotor-bg-remover-20231108143341.png", 
  //                   lactose: false, 
  //                   gluten: true,
  //                   ingredients: "Mallorn Leaves"}
  //               ]