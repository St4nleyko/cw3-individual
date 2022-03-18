<template>
    <div id="LessonsComponent">
        <div class="row" id="lessonList">
            <div class=" col-lg-3 row product card card-body" v-for="(lesson, i) in lessons"  v-bind:key="i">
                <div class="col-lg-12">
                    <p><strong>Topic</strong>: {{lesson.topic}}</p>
                    <p><strong>Location</strong>: {{lesson.location}}</p>
                    <p><strong>Price</strong>: {{lesson.price}}</p>
                    <p><strong>Space</strong>: {{lesson.space}}</p>
                    <button @click="addToCart(lesson)" v-bind:id="lesson._id" class="btn btn-success">Add to Cart</button>
                </div>                
            </div>
        </div>
    </div>
</template>

<script>
    export default {
        name: 'LessonsComponent',
        props: {
            lessons: Array
        },
        data() {
            return {
                showCheckout:false,
                searchSubject:'',       
            }
        },
        
        methods : {
            addToCart(product) {
                this.$emit('addToCart',product);
            }, 
        },
        computed: { // the Computed Property object
            cartItemCount:function() { // the propety name
            // its value is calculated when it is called
                return this.cart.length || '';
            },
        }
    }
</script>
