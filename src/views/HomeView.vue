<template>
<div>
    <div>
    <h1>Burgers</h1>
    <Burger v-for="burger in burgers"
            v-bind:burger="burger" 
            v-bind:key="burger.name"/>
    </div>
    <div id="map" v-on:click="addOrder">
    click here
    </div>
  <input type="text" v-bind:value="yourVariable" v-on:input="yourVariable = $event.target.value">
  <div>
    {{ yourVariable }}
  </div>
</div>
</template>

<script>
import Burger from '../components/OneBurger.vue'
import io from 'socket.io-client'

const socket = io("localhost:3000");

function MenuItem(name, kCal, url, gluten, lactose) {
  this.name = name;
  this.kCal = kCal;
  this.url = url;
  this.gluten = gluten;
  this.lactose = lactose;
}

const burgersArray = [
  new MenuItem("Small Burger", 300, "small-burger.jpg", false, true),
  new MenuItem("Standard Burger", 450, "standard-burger.jpg", true, true),
  new MenuItem("Large Burger", 850, "large-burger.jpg", true, false)
];

console.log(burgersArray);


export default {
  name: 'HomeView',
  components: {
    Burger
  },
  data: function () {
    return {
      burgers: burgersArray,
      yourVariable: 'Välj en burgare',
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
  #map {
    width: 300px;
    height: 300px;
    background-color: red;
  }
</style>