<template>
  <div class="block">
    <h3>{{ burger.name }}</h3>
    <img v-bind:src="burger.url" id="burgerimg">
    <ul class="centered-list">
      <li v-for="(ingredient, index) in burger.ingredients" :key="index">{{ ingredient }}</li>
      <li v-if="burger.gluten" class="allergy">Gluten</li>
      <li v-if="burger.lactose" class="allergy">Lactose</li>
    </ul>
    <p>
      <button v-on:click="decreasebutton"> - </button>
      {{ amountOrdered }} 
      <button v-on:click="increasebutton"> + </button> 
    </p>
  </div>
</template>
  
  <script>
  export default {
    name: 'OneBurger',
    props: {
      burger: Object
    },

    emits: ['orderedBurger', 'deletedBurger'],

    data: function () {
        return {
            amountOrdered: 0,
        }
    },
    methods: {
        increasebutton: function () {
            this.amountOrdered += 1;
            this.$emit('orderedBurger', { name:   this.burger.name, 
                                          amount: this.amountOrdered 
                                        }
            );
        },

        decreasebutton: function () {
            if (this.amountOrdered > 0) {
                this.amountOrdered -= 1;
            }
            this.$emit('deletedBurger', { name:   this.burger.name, 
                                          amount: this.amountOrdered 
                                        }
            );
        }, 
    }
  }
  </script>
  
  <!-- Add "scoped" attribute to limit CSS to this component only -->
  <style scoped>
    #burgerimg {
    width: 40vh;
    height: 35vh;
    overflow: hidden;
    padding: 3vh;
    }

    .allergy {
      color:brown;
      font-weight: bolder;
    }
  
    .block {
      grid-row: 2;
    }

    .centered-list {
      list-style: none;
      padding: 0; 
      text-align: center; 
      font-family: "Times New Roman", Times, serif;
    }

    .centered-list li {
      margin: 5px 0;
    }

  </style>
  