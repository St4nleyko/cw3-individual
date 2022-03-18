<template>
  <div id="app">
    <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/bootstrap@5.1.3/dist/css/bootstrap.min.css" >
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0-beta2/css/all.min.css" >
    <img alt="Vue logo" src="./assets/logo.png">
    <div v-if="showLessons">
      <button @click="openCheckout" class="btn btn-info">Checkout {{cartItemCount}}</button>
    </div>
    <div v-else>
      <button  @click="openLessons" class="btn btn-warning">Back to lesson list </button>
    </div>
    <lessonsComp v-if="showLessons" @addToCart='addToCart' :lessons="lessons" />
    <checkoutComp v-if="showCheckout"  @removeFromCart='removeFromCart' :cartItems="cart" />

  </div>
</template>

<script>
import lessonsComp from './components/lessons.vue'
import checkoutComp from './components/checkout.vue'
export default {
  name: 'App',
  components: {
    lessonsComp,
    checkoutComp
  },
  data() {
    return {
      showCheckout:false,
      showLessons:true,
      lessons:[],
      cart:[]
    }
  },            
  methods:{
    addToCart(product) {
      if(product.space > 0){
        this.cart.push(product)
        product.space--;
      }
      if(product.space <= 0){
        document.getElementById(product._id).disabled = true;
      }

    },
    removeFromCart(product) {
      this.cart.splice(this.cart.indexOf(product),1)
      product.space++;
      // if(product.space > 0){
      //   document.getElementById(product._id).disabled = false;
      // }
    
    },
    openCheckout() {
      if(this.cart.length >= 1 ){
        this.showCheckout = true;
        this.showLessons = false;
      }
    },
    openLessons(){
        this.showCheckout = false;
        this.showLessons = true;
    },
  },
  computed:{
      cartItemCount:function() 
      { // the propety name
        return this.cart.length || '0';
      },

  },
    async mounted () {
    const response = await fetch("https://cst3145mobileapp.herokuapp.com/collection/lessons");
    // const response = await fetch("http://localhost:8080/collection/lessons/");
    this.lessons = await response.json();
    // this.lessons = data;
  },

}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
