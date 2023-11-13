<template>
    <div class="block-A">
      <h3>{{ burger.name }}</h3>
      <img v-bind:src= "burger.url" id="burgerimg">
        <ul>
          <li> {{ burger.ingredients[0] }} </li>
          <li> {{ burger.ingredients[1] }} </li>
          <li> {{ burger.gluten }} </li>
          <li> {{ burger.lactose }} </li>
        </ul>
    </div>
  </template>
  
  <script>
  export default {
    name: 'OneBurger',
    props: {
      burger: Object
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

    .diet {
      color: green;
      font-weight: bolder;
    } 

s    .allergy {
      color:brown;
      font-weight: bolder;
    }
    
    #burger {
      margin: 5vh;
      padding: 2vh;
      border: 0.5vh double peru;
      text-align: center;
    }

    .Burgerselection{
      display: grid;
      grid-gap: 2em;
      grid-template-columns: 30em 30em 30em;
      background-color:lightsteelblue;
      color:azure;
    }

    .Titel-block{
      grid-column: 1 / span 3;
      grid-row: 1;
    }

    .block-A {
      grid-column: 1;
      grid-row: 2;
    }

    .block-B {
      grid-column: 2;
      grid-row: 2;
    }

    .block-C {
      grid-column: 3;
      grid-row: 2;
    }

  </style>
  