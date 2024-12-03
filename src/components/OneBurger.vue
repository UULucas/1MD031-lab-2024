<template>
  <div class="spaced">
    <section>
      <h1>{{ burger.name }}</h1>
    </section>
    <div>{{ amountOrdered }}</div>
    <img v-bind:src="burger.url" alt="span"
         title="OG Burger" style="width: 200px">
    <ul>
      <li><section class="classname">Contains <span id="idname">lactose</span></section></li>
      <li>Contains red onions</li>
      <li>Contains lactose</li>
    </ul>
    <button @click="increaseOrder">+</button>
    <button @click="decreaseOrder">-</button>
  </div>
</template>

<script>
export default {
  name: 'OneBurger',
  props: {
    burger: Object
  },
  data: function() {
    return {
      amountOrdered: 0,
    }
  },
  methods: {
    increaseOrder() {
      this.amountOrdered++;
      console.log("Updated amount: " + this.amountOrdered);
      this.$emit('orderedBurger', {
        name: this.burger.name,
        amount: this.amountOrdered
      });
    },
    decreaseOrder() {
      if (this.amountOrdered > 0) {
        this.amountOrdered--;
        console.log("Updated amount: " + this.amountOrdered);
        this.$emit('orderedBurger', {
          name: this.burger.name,
          amount: this.amountOrdered
        });
      }
    }
  },
  addBurger: function () {
    this.amountOrdered += 1;
    this.$emit('orderedBurger', { name:   this.burger.name,
          amount: this.amountOrdered
        }
    );
  },
}

</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.spaced {
  margin: 10px;
}

.classname {
  color: #000000;
}

#idname {
  text-transform: uppercase;
  font-weight: bold;
}
</style>