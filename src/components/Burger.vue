
<template>


  <div class="a">
    <h1 v-bind:key=burger.name> {{burger.name }}</h1>
    <img v-bind:src="burger.img" alt="" style="margin-right: 15px; height: 300px; width: 300px;">

    <ul>
      <li> Contains <span class="ingredients" > {{burger.kCal}}</span> calories</li>
      <li v-if="burger.lactose">  Contains Lactose</li>
      <li v-if="burger.gluten"> Contains Gluten </li>


    </ul>
    <div>
    <button v-on:click="decreaseAmount" class="numberOfBurgers">
      -
    </button>
    <button v-on:click="increseAmount" class="numberOfBurgers">
      +
    </button>
  </div>
    <br>
    {{burger.name}}: {{amountOrdered}} st

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
  


      decreaseAmount: function () {

        if(this.amountOrdered>0) {
          this.amountOrdered -= 1;

          this.$emit('orderedBurger', {name: this.burger.name, amount: this.amountOrdered});
        }
      },


      increseAmount: function () {

          this.amountOrdered+=1;
        this.$emit('orderedBurger', { name:   this.burger.name, amount: this.amountOrdered });
        
         },

      }

  },
}




</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
#burgers > div {
  padding-left: 20px;
}
.numberOfBurgers{
  width:50px;
  height:50px;
  margin-left:70px;
}




.ingredients {
  text-weight: bold;
  color: white;
}
</style>
