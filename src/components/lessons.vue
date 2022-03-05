<template>
    <div id="LessonsComponent">
            <h1>List of lessons</h1>
            <div class="row">
                <button @click="openCheckout" v-if="cartItemCount" id="checkoutButton"  class="btn col-lg-2 btn-primary">Proceed to shopping cart</button>
                <button @click="openList" v-else id="emptyCartBtn" class="btn col-lg-2 btn-disabled btn-secondary">Shopping cart empty</button>   
            </div>
            <div class="row" id="sortDiv">
                <h6 class="col-lg-1">Sort by space</h6>
                <div class="col-lg-1">
                    <button  @click="sortBySpace" class="btn btn-warning" id="sortBtn" >Ascending</button>
                </div>
                
                <div class="searchDiv col-lg-1">
                    <input type="text" v-model="searchSubject" placeholder="Search">
                    <div class="autocompleteDiv">
                        <span class="autocomplete" v-for="subject in autocomplete" v-bind:key="subject">
                            {{subject.subject}}|{{subject.location}}
                        </span>
                    </div>
                </div>
            </div>

             <div v-if="filterSearch" id="lessonListS" class="row">
                <div id="productBox" v-for="subject in filterSearch" v-bind:key="subject" class=" col-lg-4 row container lesson">
                    <div class="col-lg-6  m-auto">
                        <figure>
                            <img height="200px" width="200px"  :src="subject.img" alt="">
                        </figure>
                        <button class="btn btn-success" v-if="canAddToCart(subject.id)" @click="addToCart(subject.id)">
                            <span ><i class="fas fa-cart-plus"></i></span>
                             Add to Cart ({{subject.countItemsInCart}})
                        </button>
                        <button class="btn btn-secondary btn-disabled" v-else>Out of space</button>
                    </div>
    
                    <div class="col-lg-6  ">
                        <h2 class="subject"><i class="fas fa-book-open"></i>  {{subject.subject}}</h2>
                        <i class="fas fa-map-pin"></i><span class="location" v-text='subject.location' ></span>
                        <p class="price">Price for one space: {{subject.price}}£</p>
                        <p class="av-space">Available space: {{subject.showSpace}}</p>
                        <p class="total-price">Total price: {{subject.totalPrice}}£</p>
                    </div>
                </div>
            </div>
    


            <!-- //checkoutPage -->
        <form action="">
            <div class="row col-lg-12  container" id="checkout">
                <div class="col-lg-4">
                    <h3>Check out page</h3>
                    <span id="cartCount">({{cartItemCount}})</span>
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

                    <p>total price</p>
                    <button v-if="canPlaceOrder" @click="placeOrder" type="submit" class="btn btn-info">Place order</button>
                    <button v-else  class="btn disabled btn-secondary">Place order</button>
                </div>
                <div class="col-lg-4">
                    <h3>Order List</h3>
                    <div v-for="cartItem in cart" v-bind:key="cartItem">
                        {{key}}
                        <p>{{cartItem}} 
                            <a @click="removeFromCart(cartItem,key)" class="btn btn-danger">Remove</a>
                        </p>
                    </div>
                </div>
            </div>
        </form>
    </div>
</template>

<script>
    export default {
        name: 'LessonsComponent',
        props: {
            msg: String
        },
        data() {
            return {
                showCheckout:false,
                order:{
                firstName:"",
                secondName:"",
                phone:"",
                year:"",
                isGift:"Purchase as a gift",
                isNotGift:"Purchase for yourself"
                },
                lessons:"",
                searchSubject:'',       
                cart:[],
            }
        },
        
        methods : {
            sortBySpace(){
                let sortBtn = document.querySelector("#sortBtn");                      
                if(sortBtn.innerHTML == "Ascending"){
                    this.lessons.sort((a, b) => a.showSpace < b.showSpace ? 1 : -1);
                    sortBtn.innerHTML = "Descending";
                }
                else{
                    this.lessons.sort((a, b) => a.showSpace > b.showSpace ? 1 : -1);
                    sortBtn.innerHTML = "Ascending";
                }
                    
            },
            addToCart(subjectId) {
                this.lessons.forEach(subject => {
                    if(subject.id == subjectId && subject.space > subject.countItemsInCart){
                        subject.showSpace--;
                        subject.totalPrice+=subject.price;
                        subject.countItemsInCart++;
                        subject.available = true;
                        this.cart.push(subject.subject);
                    }
                    else{
                        subject.available = false;
                    }

                });
            },
            removeFromCart(subjectName,key){
                this.lessons.forEach(subject => {
                    if(subject.subject == subjectName ){
                        subject.showSpace++;
                        subject.totalPrice-=subject.price;
                        subject.countItemsInCart--;
                        //key represent iteration cart position of the item simple fix
                        this.cart.splice(key,1) 
                    }
            
                    });
            },

            canAddToCart(subjectId){
                for (let i = 0; i < this.lessons.length; i++) {
                    if(this.lessons[i].id == subjectId && this.lessons[i].space > 0){
                        return this.lessons[i].space >this.lessons[i].countItemsInCart;

                    }
                }
            },
            openList:() =>{
                window.location.reload()
            },
            openCheckout: () => {
                let checkoutBtn = document.querySelector('#checkoutButton');
                let lessonList = document.querySelector('#lessonListS');
                let checkoutSection = document.querySelector('#checkout');
                let sortDiv = document.querySelector('#sortDiv');

                if(checkoutBtn.innerHTML != "Back to the list of lessons"){
                    lessonList.style.display = "none";
                    checkoutSection.style.display = "inline-flex";
                    sortDiv.style.display = "none";
                    checkoutBtn.innerHTML = "Back to the list of lessons";
                }
                else{
                    lessonList.style.display = "inline-flex";
                    checkoutSection.style.display = "none";
                    sortDiv.style.display = "block";
                    checkoutBtn.innerHTML = "Proceed to shopping cart";
                }
            },
            placeOrder: (e) => { 
                alert('Order succesfully placed!')
                e.preventDefault();
            },
            async listCollection(collectionName) {
                if(collectionName == 'lessons'){
                    this.showLessons = true;
                    this.showCheckout = false;
                }
                const response = await fetch("https://cst3145mobileapp.herokuapp.com/collection/"+collectionName);
                const data = await response.json();
                this.filledData = data;
            }, 
        },
        async mounted () {
            const response = await fetch("https://cst3145mobileapp.herokuapp.com/collection/lessons");
            const data = await response.json();
            this.lessons = data;
            console.log(this.lessons)

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
            cartItemCount:function() { // the propety name
            // its value is calculated when it is called
                return this.cart.length || '';
            },

            filterSearch() {
                return this.lessons.filter( subject => {
                    if(this.searchSubject != ''){
                        // lessonList.style.display = "none";
                        return !this.searchSubject || subject.subject.toLowerCase().indexOf(this.searchSubject.toLowerCase()) > -1 ||
                            subject.location.toLowerCase().indexOf(this.searchSubject.toLowerCase()) > -1                         }
                    if(this.searchSubject == ''){
                        return true;
                    }
                })
            },
            autocomplete() {
                return this.lessons.filter( subject => {
                    if(this.searchSubject != ''){
                        return (!this.searchSubject || subject.subject.toLowerCase().indexOf(this.searchSubject.toLowerCase()) > -1)
                        ||subject.location.toLowerCase().indexOf(this.searchSubject.toLowerCase()) > -1  
                    }
                    else{
                        return "";
                    }
                })
            }
        }
    }
</script>
