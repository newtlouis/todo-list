<template>
  <div>
    <input type="text" class="todo__input" v-model="newTodo" @keyup.enter="addTodo" placeholder="Qu'avez vous à faire ?" >
    <div v-for="(todo, index) in todos" :key="todo.id" class="todo__list">

      <div class="todo__content">
        <div class="todo__content__title" v-if="!todo.editing" @click="editTodo(todo)">{{todo.title}}</div>
        <input class="todo__content__input" type="text" v-else v-model="todo.title" @blur="doneEditing(todo, index)" @keyup.enter="doneEditing(todo)" @keyup.esc="cancelEditing(todo)" v-focus>
      </div>
      <div class="todo__delete" @click="removeTodo(index)">x</div>
      </div>
  </div>
</template>

<script>
export default {
  name: 'TodoList',
  data () {
    return {
      beforeEditCache : "",
      idForTodo : 4,
      newTodo : "",
      todos : [
        {
        'id':1,
        'title' : 'Achetez des légumes',
        'completed' : false,
        'editing' : false
        },
        {
        'id':2,
        'title' : 'Appelez mamie',
        'completed' : false,
        'editing' : false

        },
        {
        'id':3,
        'title' : 'Appelez papi',
        'completed' : false,
        'editing' : false
        },
        
      ]
    }
  },
  directives: {
    focus : {
      inserted : function (el) {
        el.focus()
      }
    }
  },
  methods: {
    addTodo(){
      if(this.newTodo.trim().length == 0) {
        console.log("Rien dasn l'input")
        return
        }

      this.todos.push({
        id : this.idForTodo,
        title : this.newTodo,
        completed : false,
        editing: false
      })
      this.newTodo = ""
      this.idForTodo ++
    },

    editTodo(todo){
      this.beforeEditCache = todo.title
      todo.editing = true
    },

    doneEditing(todo, index){
      console.log(todo.title.trim().length)
      if (todo.title.trim().length == 0){
        console.log("item devient vide")
        this.todos.splice(index,1)}
      todo.editing = false;
    },

    cancelEditing(todo){
      todo.title = this.beforeEditCache;
      todo.editing = false

    },
    
    removeTodo(index){
      this.todos.splice(index,1)
    }
  }
}
</script>


<style lang="css" scoped>

.todo__input{
  width: 100%;
  padding: 10px 18px;
  font-size: 18px;
  margin-bottom: 16px;
}

.todo__input:focus{
  outline: 0;
}

.todo__list{
  margin-bottom: 10px;
  display: flex;
  justify-content: space-between;
  align-items: center;

}

.todo__delete{
  cursor: pointer;
}

.todo__delete:hover{
  color: red;
}

.todo__content__input{
  font-size: 15px;
}

</style> 


