<template>
  <div>
    <div id="myDIV" class="header">
      <h2 style="margin:5px">Hai! Add Your Daily To Do List</h2>
      <span type="button" class="text-white"
      v-on:click="logout">Logout</span>
    </div>
    <section>
      <div class="form-addList">
        <input type="text" id="input-list" v-model="newtodo" placeholder="type here to add new to-do here..">
        <span @click="addTodo()" class="add-btn">Add</span>
      </div>
      <ul id="myUL">
        <li v-for="todo in todolist" :key="todo._id">
          <span @click="editIsComplete(todo, true)"> {{ todo.task }} </span>
          <span class="close" @click="deleteTask(todo)" >x</span>
        </li>
        <li v-for="todo in completelist" :key="todo._id" class="checked">
          <span @click="editIsComplete(todo, false)"> {{ todo.task }} </span>
          <span class="close" @click="deleteTask(todo)" >x</span>
        </li>
      </ul>
    </section>
  </div>
</template>

<script>
import axios from 'axios'
import { log } from 'util';

export default {
    name: 'Login',
  data() {
    return {
      newtodo: '',
      todolist: [],
      completelist: []
    }
  },
  methods: {
    getTodo(){
      const arrTodo = []
      const arrComplete = []
      axios.get('http://localhost:3000/todo/', {
        headers: {
          token: localStorage.getItem('token')
        }
      })
        .then((response) => {
          response.data.data.map((todo) => {
            console.log(todo)
            todo.isDisabled = true
            if (todo.isComplete) {
              arrComplete.push(todo)
            } else {
              arrTodo.push(todo)
            }
          })
          this.todolist = arrTodo
          this.completelist = arrComplete
        })
        .catch((err) => {
          console.log('ini err', err)
        })
        console.log(this.todolist)
    },
    addTodo() {
      self = this

      if(this.newtodo === '') {
        alert("You must write something!")
      } else {
        axios.post('http://localhost:3000/todo/', {
          task: this.newtodo,
        }, {
          headers: {
            token: localStorage.getItem('token')
          }
        })
          .then(() => {
            self.getTodo()
          })
          .catch((err) => {
            console.log('ini err', err)
          })
      }
    },
    editIsComplete(todo, isComplete) {
      let self = this

      axios.put('http://localhost:3000/todo/', {
        id: todo._id,
        isComplete,
      }, {
        headers: {
          token: localStorage.getItem('token')
        }
      })
        .then(() => {
          self.getTodo()
        })
        .catch((err) => {
          console.log('ini err', err)
        })
    },
    editTask(todo) {
      let self = this
      console.log('masuk edit task')
      console.log('todo', todo)
      todo.isDisabled = !todo.isDisabled
      if (todo.isDisabled) {
        axios.put('http://localhost:3000/todo/task', {
          id: todo._id,
          task: todo.task
        }, {
          headers: {
            token: localStorage.getItem('token')
          },
        })
          .then(() => {
            self.getTodo()
          })
          .catch((err) => {
            console.log('ini err', err)
          })
      }
    },
    deleteTask(todo) {
      let self = this
      axios.delete(`http://localhost:3000/todo/${todo._id}`, {
        headers: {
          token: localStorage.getItem('token')
        }
      })
        .then(() => {
          self.getTodo()
        })
        .catch((err) => {
          console.log('ini err', err)
        })
    },
    logout() {
      localStorage.removeItem('token')
      this.$router.push('/login')
    }
  },
  async created() {
    this.getTodo()
  },
  mounted: () => {
    document.body.className = 'todo'
  }
}
</script>

<style scoped>
html,body{
  font-family: 'Muli', sans-serif;
}

/* Include the padding and border in an element's total width and height */
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
section{
  width: 80%;
  margin: 50px auto;
  border: 1px solid #fff;
  border-radius: 5px;
  background: white;
  box-shadow: 1px 2px 6px #efdcbc;
}
/* Style the list items */
ul li {
  cursor: pointer;
  position: relative;
  padding: 12px 8px 12px 40px;
  list-style-type: none;
  background: #fff;
  font-size: 18px;
  transition: 0.2s;
  -webkit-user-select: none;
  -moz-user-select: none;
  -ms-user-select: none;
  user-select: none;
  color: #ea526f;
  border-top: 1px solid #fbc2b5;
  text-align: left;
}
/* Darker background-color on hover */
ul li:hover {
  background: #fbc2b5;
}
ul li:last-child{
  border-bottom: 1px solid #fff;
}
ul li:last-child:hover{
  border-bottom: 1px solid #fbc2b5;
}
/* When clicked on, add a background color and strike out text */
ul li.checked {
  background: #ea526f;
  color: #fff;
  text-decoration: line-through;
}

/* Add a "checked" mark when clicked on */
ul li.checked::before {
  content: '';
  position: absolute;
  border-color: #fff;
  border-style: solid;
  border-width: 0 2px 2px 0;
  top: 10px;
  left: 16px;
  transform: rotate(45deg);
  height: 15px;
  width: 7px;
}

/* Style the close button */
.close {
  position: absolute;
  right: 0;
  top: 0;
  padding: 12px 16px 12px 16px;
}

.close:hover {
  background-color: #44bba4;
  color: white;
}

/* Style the header */
.header {
  background-color: #44bba4;
  padding: 30px 40px;
  color: white;
  text-align: center;
}

/* Clear floats after the header */
.header:after {
  content: "";
  display: table;
  clear: both;
}
.form-addList:after{
  content: "" ;
  display: table;
  clear: both;

}
/* Style the input */
input {
  border: none;
  width: 90%;
  padding: 10px 40px;
  float: left;
  font-size: 18px;
  color: #fbc2b5;
}

/*Placeholder Color*/
::placeholder{
  color: #ddd8b8;
  font-size: 16px;
  font-style: italic;
}

/* Style the "Add" button */
.add-btn {
  width: 10%;
  background: #fff;
  padding: 7px;
  color: #ea526f;
  float: left;
  text-align: center;
  font-size: 16px;
  cursor: pointer;
  transition: 0.3s;
  border: 0 solid white;
}

.add-btn:hover {
  background-color: #fbc2b5;
  border-top-right-radius: 5px;
}

</style>
