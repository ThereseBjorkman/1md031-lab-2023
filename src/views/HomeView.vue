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
                        <p>What about second breakfast?</p>
                    </div>
                    <Burger v-for="burger in burgers" :key="burger.name" :burger="burger"/>
                </div>
            </section>
            <section id="contact">
                <form>
                    <h2>Customer Information</h2>
                    <p>
                        Here you need to enter your adress and name for the delivery.
                    </p>
                    <p>
                        <label for="firstname">Full Name</label><br>
                        <input type="text" id="firstname" name="fln" required="required" placeholder="First- and Last name">
                    </p>
                    <p>
                        <label for="email">E-mail</label><br>
                        <input type="email" id="email" name="em" required="required" placeholder="E-mail address">
                    </p>
                    <p>
                        <label for="street">Street</label><br>
                        <input type="text" id="street" name="sn" placeholder="Street name">
                    </p>
                    <p>
                        <label for="house">House</label><br>
                        <input type="number" id="house" name="hn" placeholder="House number">
                    </p>
                    <p>
                        <label for="payment">Payment Method</label><br>
                        <select id="payment" name="pay" selected="Klarna">
                            <option>Silver farthing</option>
                            <option>Silver penny</option>
                            <option>Brassling</option>
                            <option>Copperlings</option>
                        </select>
                    </p>
                    <p>
                        <label>
                            <input type="radio" name="choices" value="F"> Hobbit
                        </label>
                        <label>
                            <input type="radio" name="choices" value="M"> Dwarf
                        </label>
                        <label>
                            <input type="radio" name="choices" value="O"> Elf
                        </label>
                    </p>
                </form>
            </section>
        </main>
        <button type="submit">
            <img src="https://upload.wikimedia.org/wikipedia/commons/d/d4/One_Ring_Blender_Render.png" id="ring">
            Take the ring to Mordor!
        </button>
        <hr>
        <footer>
            &copy; 2023 The Prancing Pony Inc.
        </footer>
</template>

<script>
import Burger from '../components/OneBurger.vue'
import io from 'socket.io-client'
import menu from '../assets/menu.json'

const socket = io();

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


export default {
  name: 'HomeView',
  components: {
    Burger
  },
  data: function () {
    return {
      burgers: menu
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
    }
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

  #contact {
      background-color: seagreen;
      color: black;
      margin: 5vh;
      padding: 2vh;
      border: 0.5vh double peru;
  }

  #burger {
      margin: 5vh;
      padding: 2vh;
      border: 0.5vh double peru;
      text-align: center;
  }

  button {
      margin: 2vh 5vh;
      padding: 2vh 5vh;
      border: solid black;
      cursor: pointer;
      transition: background-color 0.3s;
      text-align: center;
  }

  button:hover {
      background-color: peru;
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

</style>