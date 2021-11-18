<template>
<div>
    <div class="column">
        <label for="itemName">To do: </label>
    </div>
    <input v-model="form.name" class="input" type="text" placeholder="To do item name"><br>
    <div class="column">
        <label for="itemName">Intruction: </label>
    </div>
    <textarea v-model="form.instructions" class="input" type="text" placeholder="To do item instructions"></textarea><br>
    <input type="submit" @click = "createItem">
</div>
</template>

<script>
import axios from 'axios';
import emitter from 'tiny-emitter/instance'
export default {
  name: 'Todo',
  data() {
    return {
        form: {
            name: null,
            instructions: null,
        }
    }
  },
  methods: {
    createItem() {
      console.log('Form data', this.form);
      axios.post(`http://localhost:5000/api/todolist/create`, { 
        name: this.form.name,
        instructions: this.form.instructions 
      }).then((response) =>{
        console.log(response);
        this.form.name = null;
        this.form.instructions = null;
        emitter.emit('refresh-table');
      }).catch((error) => {
          console.log(error);
      });
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}

input[type=text]{
  width: 50%;
  padding: 12px 20px;
  margin: 8px 0;
  display: inline-block;
  border: 1px solid #ccc;
  border-radius: 4px;
  box-sizing: border-box;
}

textarea[type=text]{
    width:50%;
    padding: 12px 20px;
    border: 1px solid #ccc;
    border-radius: 4px;
    box-sizing: border-box;
}

div {
  border-radius: 5px;
  background-color: #f2f2f2;
  padding: 20px;
}

label{
    padding: 6px 6px 6px 0;
  display: inline-block;
}




</style>
