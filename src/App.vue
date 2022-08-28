<template>
  <div id="app">
    <input
      type="text"
      id="filter-user"
      v-model="filteredString"
      placeholder="Filter Users"
      @keyup="changeTable"
    />
    <div class="table">
      <div class="table__header">
        <div         
        v-for="(data, key) in usersObject[0]"
        :key="data.name">
        {{ key.toUpperCase() }}
        </div>
      </div>
      <TableRow
        v-for="(data, index) in usersObject"
        :key="`${index+data.name}`"
        :userProps="data"
        :userIndex="index"
        v-show="filteredString === '' || checkFilter(index)"
        @updatedObject="updateLocalStorage"
      />
    </div>
    <Modal v-if="showModal" />
  </div>
</template>

<script>
import TableRow from "./components/TableRow.vue";
import Modal from "./components/Modal.vue";

import axios from "axios";

export default {
  name: "App",
  components: {
    TableRow,
    Modal,
  },
  data() {
    return {
      usersObject: {},
      filteredString: "",
      filteredArr: [],
      showModal: true,
    };
  },
  async mounted() {
    if (!window.localStorage.getItem("userList")) {
      try {
        let response = await axios.get(
          "https://jsonplaceholder.typicode.com/users"
        );

        this.usersObject = response.data.map((el) => {
          const obj = { name: el.name, email: el.email, website: el.website };
          return obj;
        });
        window.localStorage.setItem(
          "userList",
          JSON.stringify(this.usersObject)
        );
        this.showModal = false;
      } catch (error) {
        // TODO: A possible feature would be to open a modal with an error message asking the user to reload the page
        console.error(error);
      }
    } else {
      this.usersObject = JSON.parse(window.localStorage.getItem("userList"));
      this.showModal = false;
    }
  },
  methods: {
    changeTable() {
      let filteredArr_ = [];

      this.usersObject.forEach((row, index) => {
        if (
          Object.values(row).some((el) =>
            el.toLowerCase().includes(this.filteredString.toLowerCase())
          )
        ) {
          filteredArr_.push(index);
        }
      });

      this.filteredArr = filteredArr_;
    },
    checkFilter(idx) {
      if (this.filteredArr.includes(idx)) {
        return true;
      } else {
        return false;
      }
    },
    updateLocalStorage(obj) {
      let objIdx = obj.index;

      Object.keys(this.usersObject[objIdx]).forEach((prop) => {
        this.usersObject[objIdx][prop] = obj.rowProperties[prop];
      });
      window.localStorage.setItem("userList", JSON.stringify(this.usersObject));
    },
  },
};
</script>

<style lang="scss">
@import url('https://fonts.googleapis.com/css2?family=Poppins:wght@300;500;700&display=swap');

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  font-family: 'Poppins', sans-serif;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
  #filter-user{
    font-size: 16px;
    border: 2px solid #dbdbdb;
    color: #252525;
    border-radius: 8px;
    padding: 8px 16px;
    margin-bottom: 32px;
    &::placeholder{
      color: #c3c3c3;
    }
    &:focus{
      outline-color: #4de09c;
      border: 2px solid transparent;
    }
  }
  .table{
    width: 80%;
    margin-left: 10%;
    &__header{
      display: flex;
      background: #4de09c;
      border-top-left-radius: 8px;
      border-top-right-radius: 8px;
      div{
        width: 30%;
        text-align: left;
        padding: 15px;
        box-sizing: border-box;
      }
    }
  }
}
</style>
