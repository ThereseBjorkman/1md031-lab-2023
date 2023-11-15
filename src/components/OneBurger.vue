<template>
  <div class="block">
    <h3>{{ burger.name }}</h3>
    <img v-bind:src="burger.url" id="burgerimg">
    <ul>
      <li v-for="(ingredient, index) in burger.ingredients" :key="index">{{ ingredient }}</li>
      <li v-if="burger.gluten" class="allergy">Gluten</li>
      <li v-if="burger.lactose" class="allergy">Lactose</li>
    </ul>
    <p>{{ amountOrdered }}</p>
  </div>
</template>
  
  <script>
  export default {
    name: 'OneBurger',
    props: {
      burger: Object
    },
    data: function () {
        return {
            amountOrdered: 0,
        }
    },
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

  </style>
  