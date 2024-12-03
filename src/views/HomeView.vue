<template>
  <header id="header">
    <div class="textImg">Burgers Online</div>
    <img src="https://d1csarkz8obe9u.cloudfront.net/posterpreviews/fast-food-design-template-486715096606ce499231c2736a6effba_screen.jpg?ts=1644097555" id="headerImg">
  </header>
  <main>
    <section id="Burgers">
      <Burger v-for="burger in burgers"
              v-bind:burger="burger"
              v-bind:key="burger.name"
              v-on:orderedBurger="addToOrder($event)"/>
    </section>
    <section class="customers">
      <h3>Customer Information</h3>
      <p>This is where you provide necessary information</p>
      <h4>Delivery Information:</h4>
      <form>
        <p>
          <label for="full name">Full name</label><br>
          <input type="text" id="full name" v-model="fullName" required="required" placeholder="First- and Last name">
        </p>
        <p>
          <label for="email">Email</label><br>
          <input type="email" id="email" v-model="email" required="required" placeholder="Email">
        </p>
        <p>
          <label for="street">Street</label><br>
          <input type="text" id="street" v-model="street" required="required" placeholder="Street">
        </p>
        <p>
          <label for="house">House</label><br>
          <input type="number" id="house" v-model="house" required="required" placeholder="House">
        </p>
        <p>
          <label for="payment">Payment method</label><br>
          <select id="payment method" v-model="paymentMethod">
            <option>Cash</option>
            <option selected="selected">Credit card</option>
            <option>Gift card</option>
            <option>Crypto</option>
          </select>
        </p>
        <p>Please select your gender</p>
        <input type="radio" id="male" value="Male" v-model="gender" required="required">
        <label for="male">Male</label><br>
        <input type="radio" id="female" value="Female" v-model="gender" required="required">
        <label for="female">Female</label><br>
        <input type="radio" id="dnwts" value="Do not wish to specify" v-model="gender" required="required">
        <label for="male">Do not wish to specify</label><br>
      </form>
      <button type="button" id="orderButton" v-on:click="submitOrder">
        <img src="https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcQPucVEkm69jXUSPgj6AmN4O13flENs9v-L6Q&s" style="width: 20px">
        Order
      </button>
    </section>
  </main>

  <footer>
    <hr>
    UU Burgers &copy;
  </footer>
</template>

<script>
import Burger from '../components/OneBurger.vue'
import io from 'socket.io-client'
import menu from '../assets/menu.json'

const socket = io("localhost:3000");

function MenuItem(name, kCal, url, gluten, lactose) {
  this.name = name;
  this.kCal = kCal;
  this.url = url;
  this.gluten = gluten;
  this.lactose = lactose;
}

const burgersArray = [
    // ...menu kmr göra att varje burgare i jsonen blir ett eget objekt i burgersArray
    ...menu
  //new MenuItem("KFC Burger", 300, "https://static.vecteezy.com/system/resources/thumbnails/022/911/694/small_2x/cute-cartoon-burger-icon-free-png.png", false, true),
  //new MenuItem("OG Burger", 450, "https://www.burgerandsauce.com/wp-content/uploads/2021/02/burger-and-suace-chicken-burger-3000px-2.1.png", true, true),
  //new MenuItem("Smashed Burger", 850, "https://lh5.googleusercontent.com/proxy/D8fDyPoDbzDV9mgGpTknMCObH456uxrslMpDm_WQlc3yKUxxcpvtGQMjdILDKpmtWcMFgedwfudpNbugw5bmBT1CeyADbGgkDbMVtSb16K4D", true, false)
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
      fullName: '',
      email: '',
      street: '',
      house: '',
      paymentMethod: '',
      gender: '',
      orderedBurgers: {},
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
    },
    addToOrder: function (event) {
      if (event.amount > 0) {
        this.orderedBurgers[event.name] = event.amount;
      }
      else {
        delete this.orderedBurgers[event.name];
      }
      console.log(this.orderedBurgers); // För debugging
    },
    submitOrder: function () {
      console.log("Order Details:");
      console.log("Name:", this.fullName);
      console.log("Email:", this.email);
      console.log("Street:", this.street);
      console.log("House:", this.house);
      console.log("Payment Method:", this.paymentMethod);
      console.log("Gender:", this.gender);
    }
  },
}
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Agbalumo&family=Cormorant:wght@700&display=swap');
@import 'https://fonts.googleapis.com/css?family=Pacifico|Dosis';
body {
  font-size: 12pt;
}

p {
  /* color: white; */
}

h1 {
  font-family: 'Inter Semi Bold', sans-serif;
  font-size: 36pt;
}
main, header, footer, nav ul {
  max-width: 40rem;
  margin: 0 auto 0 auto;
}
main {
  background-color: bisque;
}

/* nav ul li {
    display: inline-block;
    background-color: grey;
    padding: 1em;
    margin: 1em;
} */


#header {
  position: relative;
  width: 100%;
  max-width: 600px;
  margin: auto;
  overflow: hidden;
}

#headerImg {
  width: 100%;
  height: auto;
  display: block;
  opacity: 0.5;
  padding: 1em;
}

header h1 {
  width:40rem;
  margin: 0 auto;
  text-align: center;
}

nav ul {
  display: grid;
  grid-template-columns: repeat(auto-fill, 9.25em);
  gap: 1em;
  padding: 0;
}

nav li {
  display: block;
  background-color: grey;
  padding: 1em;
}

.Very-good {
  color: green;
}

.Master {
  color: green;
  font-weight: bold;
}

.classname {
  color: #000000;
}

#idname {
  text-transform: uppercase;
  font-weight: bold;
}


.customers {
  color: #ffffff;
  background-color: black;
  margin: 20px;
}

button:hover {
  background-color: blanchedalmond;
  color: black;
  cursor: pointer;
}

#Burgers {
  display: grid;
  grid-template-columns: repeat(auto-fill, 11em);
  margin: 20px;
  border: 10px solid #B99976;
  padding: 10px;
}

#order\ button {
  margin: 5px;
}

.spaced {
  margin: 10px;
}

.textImg {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  color: black;
  font-size: 2rem;
  font-weight: bold;
  text-align: center;
  pointer-events: none;
}
</style>