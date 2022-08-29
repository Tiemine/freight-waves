<template>
  <!-- Here I'm using a v-for for each element received on this component, for example, name and email-->
  <div class="table-row" :class="{'even' : userIndex % 2 == 0}">
    <div
      v-for="(prop, key) in userProps"
      :key="prop"
      :class="`table-row__${key}`"
    >
      <span v-if="!editing">{{ prop }}</span>
      <span v-if="editing">
        <!-- I concat the key plus index so all the ids are unique-->
        <input type="text" :id="`${key+userIndex}`" :name="key" :value="prop" />
      </span>
    </div>
    <div class="table-row__edit-button">
      <button v-if="!editing" @click="editUser">Edit user</button>
      <button v-if="editing" @click="saveUser">Save user</button>
    </div>
  </div>
</template>

<script>
export default {
  name: "TableRow",
  props: {
    userProps: Object,
    userIndex: Number,
  },
  data() {
    return {
      editing: false,
    };
  },
  methods: {
    editUser() {
      this.editing = true;
    },
    saveUser() {
      // This is a temporary object I created to update and send it to the parent component to update the store
      let updatedObj = {
        index: this.userIndex,
        rowProperties: this.userProps,
      };
      this.editing = false;
      Object.keys(this.userProps).forEach((el) => {
        // I run over all the inputs on the row and get the value typed on them to send to my temporary object
        const currentInput = document.querySelector(`#${el+this.userIndex}`);

        updatedObj.rowProperties[el] = currentInput.value;
      });
      this.$emit("updatedObject", updatedObj);
    },
  },
};
</script>

<style scoped lang="scss">
.table-row {
  display: flex;
  align-items: center;
  width: 100%;
  box-sizing: border-box;
  font-family: 'Poppins', sans-serif;
  font-weight: 300;
  height: 50px;
  color: #c3c3c3;
  padding-left: 15px;
  input{
    font-size: 16px;
    border: 2px solid #dbdbdb;
    color: #252525;
    border-radius: 8px;
    padding: 4px 8px;
    &:focus{
      outline-color: #4de09c;
      border: 2px solid transparent;
    }
  }
  &__name,
  &__email,
  &__website {
    width: 30%;
    padding: 5px 0;
    text-align: left;
  }
  &__edit-button {
    width: 10%;
    button {
      cursor: pointer;
      border: 0;
      color: #252525;
      background: #4de09c;
      border-radius: 50px;
      padding: 6px 12px;
      font-weight: 500;
    }
  }
  &:hover{
    color: #252525;
    transition-duration: 0.5s;
    div:first-of-type{
      font-weight: 500;
    }
  }
  &.even{
    background-color: rgb(249 249 249);
  }
}
</style>
