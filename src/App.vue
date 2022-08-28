<template>
  <div id="app">
    <input type="text" id="filter-user" v-model="filteredString" placeholder="Filter Users" @keyup="changeTable"/>
    <TableRow 
      v-for="(data, index) in usersObject" 
      :key="data.name" :userProps="data" 
      :userIndex="index" 
      v-show="filteredString === '' || checkFilter(index)"
      @updatedObject="updateLocalStorage"
      />
    <Modal v-if="showModal"/>
  </div>
</template>

<script>
import TableRow from './components/TableRow.vue'
import Modal from './components/Modal.vue'

import axios from 'axios'

export default {
  name: 'App',
  components: {
    TableRow,
    Modal
},
  data() {
    return {
      usersObject: {},
      filteredString: '',
      filteredArr: [],
      showModal: true
    }
  },
  async mounted() {
    if (!window.localStorage.getItem('userList')) {
      try{
        let response = await axios.get('https://jsonplaceholder.typicode.com/users')
  
        this.usersObject = (response.data).map( el => {
          const obj = {name: el.name, email: el.email, website: el.website};
          return obj;
        });
        window.localStorage.setItem('userList', JSON.stringify(this.usersObject))
        this.showModal = false;
      }
      catch(error){
        console.error(error)
      }
    }
    else {
      this.usersObject = JSON.parse(window.localStorage.getItem('userList'));
      this.showModal = false;
    }
  },
  methods: {
    changeTable() {
      let filteredArr_ = [];

      this.usersObject.forEach((row, index) => {
        if (Object.values(row).some((el) => el.toLowerCase().includes(this.filteredString.toLowerCase()))){
          filteredArr_.push(index)
        }
      })

      this.filteredArr = filteredArr_;
    },
    checkFilter(idx) {
      if (this.filteredArr.includes(idx)) {
        return true
      } else {
        return false
      }
    },
    updateLocalStorage(obj){
      let objIdx = obj.index;

      Object.keys(this.usersObject[objIdx]).forEach(prop => {
        this.usersObject[objIdx][prop] = obj.rowProperties[prop]
      })
      window.localStorage.setItem('userList', JSON.stringify(this.usersObject))
    }
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
