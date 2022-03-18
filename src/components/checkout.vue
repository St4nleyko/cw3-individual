<template>
    <div id="checkoutComponent">
        <!-- //checkoutPage -->
        <form action="">
            <div class="row col-lg-12  container" id="checkout">
                <div class="col-lg-4">
                    <h3>Check out page</h3>
                    <div class="form-group">
                        <label for="firstName">First Name:
                            <input required v-model="order.firstName" type="text">
                    </label>
                    </div>
                    <div class="form-group">
                        <label for="lastName">Last Name:
                            <input required v-model="order.lastName" type="text">
                        </label>
                    </div>
                    <div class="form-group">
                        <label for="phone">Phone:
                            <input id="phone" required v-model="order.phone" type="text">
                        </label>
                    </div>
                    <div class="form-group">
                        <label for="year">Year of study:
                            <select v-model="order.year">
                                <option value="">-</option>
                                <option>1</option>
                                <option>2</option>
                                <option>3</option>
                                <option>4</option>
                                <option>5</option>
                                <option>6</option>
                            </select>
                        </label>
                    </div>
                </div>
                <div class="col-lg-4">
                    <h3>Order Summary</h3>
                    <p>Name: {{order.firstName}} {{order.lastName}} </p>
                    <p>Phone: {{order.phone}}</p>
                    <p>Year of study: {{order.year}} </p>

                    <p>Total price: {{order.totalPrice}}</p>
                    <button v-if="canPlaceOrder" @click="placeOrder" type="submit" class="btn btn-info">Place order</button>
                    <button v-else  class="btn disabled btn-secondary">Place order</button>
                </div>
                <div class="col-lg-4">
                    <h3>Order List</h3>
                    <div class="" v-for="(cartItem, i) in cartItems" v-bind:key="i">
                        <div class=""><p>Location: {{cartItem.location}}</p> <p>Topic: {{cartItem.topic}}</p> <p>Space: {{cartItem.space}}</p>    </div>
                         <a @click="removeFromCart(cartItem)" class=" btn btn-danger">Remove</a>
                    </div>
                </div>
            </div>
        </form>
    </div>
</template>

<script>
    export default {
        name: 'checkoutComponent',
        props: {
            cartItems: Array
        },
        data() {
            return {
                order:{
                    firstName:"",
                    secondName:"",
                    phone:"",
                    year:"",
                    isGift:"Purchase as a gift",
                    isNotGift:"Purchase for yourself",
                    totalPrice:0,
                },
            }
        },
        
        methods : {
            removeFromCart(product) {
                this.$emit('removeFromCart',product);
            },

            placeOrder: (e) => { 
                alert('Order succesfully placed!')
                e.preventDefault();
            },
        },

        computed: { // the Computed Property object
            canPlaceOrder:function() {
                console.log(this.order.phone)

                if ((this.order.firstName && this.order.lastName && this.order.phone) && (!isNaN((this.order.phone)))) {
                    return true;
                    }
                else{
                    return false;
                }
            },

        }
    }
</script>
