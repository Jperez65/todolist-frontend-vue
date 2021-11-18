<template>
  <h3>To-do:</h3>
  <div>
   <table id="toDo">
    <thead>
    <tr>
      <th>ID</th>
      <th>Name</th>
    </tr>
    </thead>
    <tbody>
      <tr v-for="(todo, index) in toDoList" 
      :key="index">
      <td>{{ todo.id }}</td>
      <td>{{ todo.name }}</td>
      <td> {{ todo.instructions}}</td>
      <div> 
      <button 
      @click="deleteItem(todo.name)"
      >delete</button>
      </div>
      </tr>
    </tbody>
   </table>
  </div>
</template>

<script>
import axios from 'axios';
import emitter from 'tiny-emitter/instance'

export default {
  name: 'Todo',
  data() {
    return {
      toDoList: [],
    };
  },
  mounted () {
    this.getToDoList();
    // this gets called on redering of the vue componet
    emitter.on('refresh-table', () => {
      console.log('refresh was called');
      this.getToDoList();
    });
    console.log('functions was called');
  },
  methods: {
    // method to pull data into this.todo list 
    getToDoList() {
      // api call here
      axios.get('http://localhost:5000/getall/').then((response) => {
        console.log('response data ', response);
        this.toDoList = response.data;
      }).catch((error) => {
        console.log(error);
      });
      console.log(this.toDoList);
    },
    deleteItem(name){
      axios.delete(`http://localhost:5000/api/todolist/delete/${name}`).then((response) => {
        console.log(response);
        this.getToDoList();
      }).catch((error) =>  {
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

#toDo{
  font-family: Arial, Helvetica, sans-serif;
  border-collapse: collapse;
  width: 100%;
}

#toDo td, #customers th {
  border: 1px solid #ddd;
  padding: 8px;
}

#toDo tr:nth-child(even){background-color: #f2f2f2;}

</style>
