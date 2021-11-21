
<template>

  <header>
    <div id="header-layout">
      <div id="header-bild">
        <img src="https://static.thatsup.co/content/img/place/t/u/tugg-burgers-uppsala-0.jpg">
      </div>
      <div id="header-text"><h1>Stinas Burgers Online</h1></div>
    </div>

  </header>
  <main>
    <section id="burgers">
      <div>
        <h2>Select burger</h2>
        <p> This is where you choose your burger</p>
        <div class="wrapper">
          <Burger v-for="burger in burgers"
                  v-bind:burger="burger"
                  v-bind:key="burger.name"
                  v-on:orderedBurger="addToOrder($event)"/>


          <br>



        </div>
      </div>
    </section>
    <section id="order-information">
      <div>
        <h3>Customer information:</h3>
        <p>Orders take up to 10 hours</p>



        <h4>Delivery information:</h4>
        <form>
          <p>
            <label for="Full name">Full name</label><br>
            <input type="text" id="Full name" v-model="fn" required="required" placeholder="First-and last name">
          </p>
          <p>
            <label for="email">Email</label><br>
            <input type="email" id="email" v-model="em" required="required" placeholder="E-mail address">
          </p>


          <p>
            <label for="payment">Payment Options</label><br>
            <select v-model="selected">
              <option>Credit card</option>
              <option>Swish</option>
              <option>Klarna</option>
              <option>Giftcard</option>


            </select>
          </p>


          <legend>Gender</legend>
          <input type="radio" v-model="Gender" value="Female">Female<br>
          <input type="radio" v-model="Gender" value="Male">Male<br>
          <input type="radio" v-model="Gender" value="Do not wish to provide" checked>Do not wish to provide<br>
          <br>


        </form>
        <div id="mapcontainer">
          <div id="map" v-on:click="setLocation">
            <div id="dots">
              <div v-bind:style="{left: location.x + 'px', top: location.y + 'px'}">
                T
              </div>
            </div>
          </div>
        </div>
      </div>
    </section>
    <button v-on:click=submitorder>
      <img src="https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcQxPIGKfNX0Udm_jN5gCCsoAQsUc_bSeUunJQ&usqp=CAU" style="height: 15px; width: 15px;">
      Place my order!
    </button>


  </main>
  <hr>
  <footer>
    <p>&copy; Stinas burgers</p>
  </footer>

</template>

<script>
import Burger from '../components/Burger.vue'
import io from 'socket.io-client'
import menu from '../assets/menu.json'
const socket = io();



/*function MenuItem(name,kcal,lactose,gluten,url){
  this.name = name;
  this.kcal = kcal;
  this.lactose = lactose;
  this.gluten = gluten;
  this.url=url;
}
let burger_array=[
    new MenuItem("Local", "600", false,false, "https://th.bing.com/th/id/OIP.K7n32ni4lc12pS8YMZfz8gHaGj?w=204&h=180&c=7&r=0&o=5&dpr=2&pid=1.7"),
    new MenuItem("Texmex", "700", false, true, "https://th.bing.com/th/id/OIP.8M05Gwoh81inkKbptaNBVQHaE8?w=254&h=180&c=7&r=0&o=5&dpr=2&pid=1.7"),
    new MenuItem("Vegan", "800", true, false, "https://th.bing.com/th/id/OIP.8Ud7AvxOXcBgm9USxbAFBAHaE0?w=242&h=180&c=7&r=0&o=5&dpr=2&pid=1.7")
]*/
const burger_array=menu;
console.log(burger_array)
export default {
  name: 'Home',
  components: {
    Burger
  },
  data: function () {



    return {
      burgers: burger_array,
      fn:"",
      em:"",
      selected:"Klarna",
      /* paym:"",*/
      Gender:"Do not wish to provide",
      orderedBurgers:{},
      location: {x: 0, y: 0}


    }
  },
  methods: {
    getOrderNumber: function () {
      return Math.floor(Math.random()*100000);
    },
    addToOrder: function (event) {
     return this.orderedBurgers[event.name] = event.amount;


    },
    /*addOrder: function (event) {
      var offset = {x: event.currentTarget.getBoundingClientRect().left,
                    y: event.currentTarget.getBoundingClientRect().top};

      this.location={x: event.clientX - 10 - offset.x, y: event.clientY - 10 - offset.y}*/

    submitorder: function() {
      /*console.log(this.fn,this.paym,this.Gender,this.em,this.orderedBurgers)*/
      socket.emit("addOrder", {
        orderId: this.getOrderNumber(),
        details: {x: this.location.x, y: this.location.y}, orderItems: this.orderedBurgers,
        form: {fn: this.fn, em: this.em, Gender: this.Gender, selected: this.selected},

      }
      )},
    setLocation: function(event){
      var offset = {x: event.currentTarget.getBoundingClientRect().left, y: event.currentTarget.getBoundingClientRect().top};
      this.location={x: event.clientX - 10 - offset.x, y: event.clientY - 10 - offset.y}
    }
  }
}
</script>

<style>



  #header-layout {
    height:200px;
    width: 1204px;
    overflow:hidden;


  }
  #header-bild{
    opacity: 0.75;
  }
  #header-text{
    position:absolute;
    margin-top:-70em;
    padding-left: 50px;
    padding-top:80px;
  }
  body {
    font-family: "Times New Roman";

  }

  #burgers {
    background-color:black;
    color:white;
    border: 2px dotted white;
    padding-bottom: 30px;

  }

  button:hover {
    background-color:pink;
  }
  header h1 {
    margin-top:55px;
  }
  button {
    margin-top:25px;
  }
  h2 {
    margin-top: 30px;
  }
  div {
    padding-right:30px;
  }

  #order-information{

    background-color:pink;
    color:blue;
    border: 2px dotted blue;
    margin-top: 20px;
  }
  #order-information > div {
    padding-left: 20px;
  }
  #burgers > div {
    padding-left: 20px;
  }



  .wrapper {
    display: grid;
    /*grid-gap: 70px;*/
    grid-template-columns: auto auto auto;
    background-color: #444;
    color: #fff;
    border-radius: 50px;
    border: 2px dotted white;
    padding: 20px;
    font-size: 150%;

  }

  #map {
    width: 1920px;
    height: 1078px;
    background: url("/img/polacks.jpg");
  }
  #mapcontainer{
    width: 800px;
    height: 450px;
    overflow:scroll;
    margin-bottom: 20px;
  }

  #dots div {
    position: relative;
    background: black;
    color: white;
    border-radius: 10px;
    width:20px;
    height:20px;

  }
  #dots {
    position: relative;
    margin: 0;
    padding: 0;
    background: url(/img/polacks.jpg);
    background-repeat: no-repeat;
    width:1920px;
    height: 1078px;
    cursor: crosshair;
  }

</style>
