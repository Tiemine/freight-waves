<template>
  <div class="table-row">
    <div v-for="(prop, key) in userProps" :key="prop" :class="`table-row__${key}`">
      <span v-if="!editing">{{ prop }}</span>
      <span v-if="editing">
        <input type="text" :id="key" :name="key" :value="prop"/>
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
  name: 'TableRow',
  props: {
    userProps: Object,
    userIndex: Number,
    default: null
  },
  data() {
    return {
      editing: false
    }
  },
  methods: {
    editUser(){
      this.editing = true;
    },
    saveUser(){
      let updatedObj = {
        index: this.userIndex,
        rowProperties: this.userProps
      }
      this.editing = false;
      Object.keys(this.userProps).forEach(el => {
        let currentInput = document.querySelector(`#${el}`);

         updatedObj.rowProperties[el] = currentInput.value;
      })
      this.$emit('updatedObject', updatedObj);
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
.table-row{
  display: flex;
  width: 100%;
  &__name,
  &__email,
  &__website{
    width: 30%;
  }
  &__edit-button{
    width: 10%;
    button{
      cursor: pointer;
    }
  }
}
</style>
