<template>
    <div class="" id="oldApp">
        <div class="row btn-group col-lg-6">
            <button class="btn btn-info col-lg-3" @click="listCollection('lessons')">Get all lessons</button>
            <button class="btn btn-success col-lg-3" @click="listCollection('users')">Get users</button>
            <button class="btn btn-warning col-lg-3" @click="listCollection('orders')">Get orders</button>
            <button class="btn btn-danger col-lg-3" @click="getFile()">Get File</button>
        </div>
        <h4>Select data</h4>
        <div class="row">
            <div v-if="showLessons">    
                <div class=" col-lg-3 row product card card-body" v-for="item in filledData " v-bind:key="item">
                    <div class="col-lg-12">
                        <p><strong>ID</strong>: {{item._id}}</p>
                        <p><strong>Topic</strong>: {{item.topic}}</p>
                        <p><strong>Location</strong>: {{item.location}}</p>
                        <p><strong>Price</strong>: {{item.price}}</p>
                        <p><strong>Space</strong>: {{item.space}}</p>
                    </div>
                </div>
            </div>
            <div v-if="showUsers">    
                <div class=" col-lg-3 row product card card-body" v-for="item in filledData " v-bind:key="item">
                    <div class="col-lg-12">
                        <p><strong>Name</strong>: {{item.name}}</p>
                        <p><strong>Email</strong>: {{item.email}}</p>
                        <p><strong>Password</strong>: {{item.password}}</p>
                    </div>
                </div>
            </div>
            <div v-if="showOrders">    
                <div  class=" col-lg-3 row product card card-body" v-for="item in filledData " v-bind:key="item">    
                    <div class="col-lg-12">
                        <p><strong>Name</strong>: {{item.name}}</p>
                        <p><strong>Phone</strong>: {{item.phone_number}}</p>
                        <p><strong>Number of space</strong>: {{item.number_of_space}}</p>
                        <p><strong>lessonID</strong>: {{item.lesson_id}}</p>
                    </div>
                </div>
            </div>
            
        </div>
        <div v-if="showLessons">
            <h2>Add Data</h2>
            <div class="form-group">
                <label for="topic"><b>Topic</b>:
                    <input class="form-group" type="text" v-model="toPostData.topic">
                </label>
                <label for="topic"><b>Location</b>:
                    <input class="form-group" type="text" v-model="toPostData.location">
                </label>
                <label for="topic"><b>Space</b>:
                    <input class="form-group" type="number" v-model="toPostData.space">
                </label>
                <label for="topic"><b>Price</b>:
                    <input class="form-group" type="number" v-model="toPostData.price">
                </label>                    
            </div>
            <button class="btn add-button btn-success"  @click="addToCollection('lessons')">Add Lesson</button>
        </div>
        <div v-if="showUsers">
            <h2>Add Data</h2>
            <div class="form-group">
                <label for="topic"><b>Name</b>:
                    <input class="form-group" type="text" v-model="toPostData.username">
                </label>
                <label for="topic"><b>Email</b>:
                    <input class="form-group" type="email" v-model="toPostData.email">
                </label>
                <label for="topic"><b>Password</b>:
                    <input class="form-group" type="password" v-model="toPostData.password">
                </label>              
            </div>
            <button class="btn add-button btn-success"  @click="addToCollection('users')">Add User</button>
        </div>
        <div v-if="showOrders">
            <h2>Add Data</h2>
            <div class="form-group">
                <label for="topic"><b>Name</b>:
                    <input class="form-group" type="text" v-model="toPostData.name">
                </label>
                <label for="topic"><b>Phone Number</b>:
                    <input class="form-group" type="text" v-model="toPostData.phone_number">
                </label>
                <label for="topic"><b>Number of space</b>:
                    <input class="form-group" type="number" v-model="toPostData.number_of_space">
                </label>
                <label for="lesson"><b>Select lesson</b>:
                    <select v-model="selected" name="" id=""> 
                        <option class="option-lesson" v-for="item in availableLessons" v-bind:key="item._id">
                            {{item.topic}}
                            </option>
                    </select>
                </label>                    
            </div>
            <button class="btn add-button btn-success"  @click="addToCollection('orders')">Submit order</button>
        </div>
    </div>
</template>

<script>
export default {
    name: 'oldApp',
    props: {
        msg: String
    },
    data() {
        return {
            filledData:null,
            showLessons:false,
            showUsers:false,
            showOrders:false,
            selected:'',    
            availableLessons:null,
            toPostData:{
                topic:"",
                price:0,
                space:0,
                location:"",
                name:"",
                phone_number:"",
                number_of_space:1,
                lesson_id:"",
                username:"",
                email:"",
                password:""
            }
        }
    },
    methods : {
        // async getFile() {
        //     const response = await fetch("https://cst3145mobileapp.herokuapp.com/image.img");
        //     const data = await response.json();
        // },
        async listCollection(collectionName) {
            if(collectionName == 'lessons'){
                this.showLessons = true;
                this.showUsers = false;
                this.showOrders = false
            }
            if(collectionName == 'users'){
                this.showLessons = false;
                this.showUsers = true;
                this.showOrders = false
            }
            if(collectionName == 'orders'){
                this.showLessons = false;
                this.showUsers = false;
                this.showOrders = true
            }
            const response = await fetch("https://cst3145mobileapp.herokuapp.com/collection/"+collectionName);
            const data = await response.json();
            this.filledData = data;
        },

        async addToCollection(collectionName){
            let newItem;
            if(collectionName == 'lessons'){
                newItem= {
                    location: this.toPostData.location,
                    price: this.toPostData.price,
                    //numbers need verification
                    space: this.toPostData.space,
                    topic: this.toPostData.topic
                }
            }
            if(collectionName == 'users'){
                newItem= {
                    name: this.toPostData.username,
                    email: this.toPostData.email,
                    password: this.toPostData.password
                }   
            }
            if(collectionName == 'orders'){
                newItem= {
                    name: this.toPostData.name,
                    phone_number: this.toPostData.phone_number,
                    lesson_id: this.selected,
                    number_of_space: this.toPostData.number_of_space
                }
                this.updateNumberOfSpace();
            }

            fetch('https://cst3145mobileapp.herokuapp.com/collection/'+collectionName+'/',{
                method: 'POST',
                headers: {
                    'Content-type': 'application/json',
                },
                body: JSON.stringify(newItem),
            })
            .then(response => response.json())
            .then(response => {
                console.log(response);
                window.location.reload()
            });
        },
        async updateNumberOfSpace() {
            let id = this.selected
            let space = this.toPostData.number_of_space
            
            fetch('https://cst3145mobileapp.herokuapp.com/collection/lessons/'+id+'/',{
                method: 'PUT',
                headers: {
                    'Content-type': 'application/json',
                },
                body: JSON.stringify({space:space}),
            })
            .then(response => response.json())
            .then(response => {
                console.log(response);
            });
        },
    },
    async mounted () {
        const response = await fetch("https://cst3145mobileapp.herokuapp.com/collection/lessons");
        const data = await response.json();
        this.availableLessons = data;
    }
}
</script>
