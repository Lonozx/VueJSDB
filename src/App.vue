<template>

  <div class="container__create">
<h2>Add new user</h2>
  <input type="text" v-model="first_name" name="first_name" placeholder="Name please">
  <br>
  <input type="email" v-model="email" name="email" placeholder="Email please">
  <button type="submit" @click="handleAdd">Add user</button>
</div>
<div class="apiNext ">
  <h2>Users</h2>
  <div class="container__searchbar">
  <button @click="bandle">Bandle by name</button>
    <input v-model="search" type="text" placeholder="Name you want to find..." name="search">
    <button @click="doSearch">Search</button>
  </div>
  <ul v-for="item in list" :key="item.id">
    
    
   <div class="card">
    <div class="card-slice">
    <li @click="handleShow">{{ item.first_name }}</li>
    
    <li>{{ item.email }} </li>
  </div>
    <li>
      <img class="avatar" :src="item.avatar" alt="user avatar">
    </li>
    <div v-if="showModal">
      <Modal :header="item.first_name" :text="item.email" :avatar="item.avatar" :last_name="item.last_name" :number="380636154732" :address="1" @close="toggleModal"/>
    </div>
    <button @click="handleDelete(item.id)">Delete</button>
  </div>
  </ul>
  <!-- <h2>{{quote}}</h2> -->
  
</div>

</template>

<script>
import Modal from './components/Modal.vue'
import axios from 'axios'
import {ref} from 'vue'
export default {
  async mounted(){
  this.handle();
  },
  name: 'App',
  components: {Modal},
  data(){
    return{
      email: '',
      first_name: '',
      name:'',
      list:[],
      showModal: false,
    }
  },
  computed: {
    isValidEmail() {
      return /^[^@]+@\w+(\.\w+)+\w$/.test(this.email);
    }
  },
  watch: {
  search(value) {
    this.doSearch(value);
  }
},
  methods:{
    async handleAdd(event){
      if(this.isValidEmail && this.first_name!=''){
      let result = await axios.post("https://reqres.in/api/users/register",
      {
        first_name:this.first_name,
        email:this.email,
      })
      
      .then(res=>res.data);
      console.log(result);
      alert("Added new user. Check the console")
      event.preventDefault();
      this.first_name = '';
        this.email = '';
      } else alert('Email or name is no valid')
    },
   
    handleDelete(id){
  
      if(confirm('Are you sure you want to delete this user?')){
         axios.delete(`https://reqres.in/api/users/${id}`)
        .then(response => {
           this.list = this.list.filter(user => user.id !== id)})
        }
    },
   async bandle(){
      // this.list = this.list.filter(user => user=new Array(user))
    },
    async doSearch(value){
    axios
     .get('https://reqres.in/api/users' + this.search)
     .then((response) => {this.list = response.data})
     .catch(e => console.log(e));
  },
    
    handleShow(){

      this.showModal=!this.showModal;
    },
   
    async handle(){
    let result = await axios.get('https://reqres.in/api/users/')
    console.warn(result.data.data);
    this.list = result.data.data;
    },
    toggleModal(){
      this.showModal=!this.showModal;
    }
  }
}
</script>

<style>
#app {
  background: #e9e6f5;
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

.container__create{
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  gap: 10px;
  font-size: 20px;
}
.container__searchbar{
  display: flex;
  gap: 20px;
  justify-content: center;
}
.card{
  border: 1px black solid;
  border-radius: 10px;
  display: flex;
  gap: 20px;
  justify-content: center;
  align-items: center;
  padding: 10px;
  
}
.card-slice{
  display: flex;
  flex-direction: column;
  gap: 20px;
}
button{
  border: none;
  border-radius: 10px;
  padding:10px;
  font-size: 20px;
  background: #beb1ff;
  cursor: pointer;
}
button::hover{
  
}
input{
  background: transparent;
  border: 0.1px solid;
  border-radius: 10px;
  padding: 10px;
}
ul{
  list-style-type: none;
  font-size: 20px;
}
</style>
