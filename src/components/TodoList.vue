<template>
  <div>
    <input type="text" class="todo__input" v-model="newTodo" @keyup.enter="addTodo" placeholder="Qu'avez vous à faire ?" >
    <div v-for="(todo, index) in todosFiltered" :key="todo.id" class="todo__list">

      <div class="todo__content">
        <input type="checkbox" v-model="todo.completed">
        <div class="todo__content__title" :class="{todo__completed : todo.completed}" v-if="!todo.editing" @click="editTodo(todo)">{{todo.title}}</div>
        <input class="todo__content__input" type="text" v-else v-model="todo.title" @blur="doneEditing(todo, index)" @keyup.enter="doneEditing(todo)" @keyup.esc="cancelEditing(todo)" v-focus>
      </div>
      <div class="todo__delete" @click="removeTodo(index)">x</div>
    </div>
    <div class="bottom__container">
      <div><label><input type="checkbox" :checked="!anyRemaining" @change="checkAllTodos">Tous selectionnés</label></div>
      <div>{{remaining}} {{textRemaining}}</div>
    </div>
    <div class="bottom__container">
      <div>
        <button :class="{active : filter == 'all'}" @click="filter = 'all'">Tous</button>
        <button :class="{active : filter == 'completed'}" @click="filter = 'completed'">Achevées</button>
        <button :class="{active : filter == 'notCompleted'}" @click="filter = 'notCompleted'">Inachevées</button>
      </div>
      <div>
        <button v-if="isThereCompletedTodos" @click="deleteCompletedTodos()">Supprimer les tâches achevées</button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'TodoList',
  data () {
    return {
      filter : 'all',
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
  computed: {
    todosFiltered(){
      if (this.filter == "all") {return this.todos}
      else if (this.filter =="completed"){
        return this.todos.filter(todo => todo.completed)
      }
      else {
        return this.todos.filter(todo => !todo.completed)
      }
    },
    remaining() {
      let remainingTodo = this.todos.filter(todo => !todo.completed).length
      if(remainingTodo > 0){return remainingTodo}
      else { return }
    },
    textRemaining(){
      if (this.remaining > 1) {return "tâches restantes"}
      else if(this.remaining == 1) {return "tâche restante"}
      else {return "Aucune tâche à faire"}
    }
    ,
    anyRemaining(){
      let remainingTodo = this.todos.filter(todo => !todo.completed).length
      return remainingTodo != 0;
    },
    isThereCompletedTodos(){
      if (this.todos.filter(todo => todo.completed).length != 0){return true}
      else {return false}
    }
  }
  ,
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
    },
    checkAllTodos(){
      this.todos.forEach(todo => todo.completed = event.target.checked)
    },
    deleteCompletedTodos(){
      this.todos = this.todos.filter(todo => !todo.completed)
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
  outline: none;
}

.todo__list{
  margin-bottom: 10px;
  display: flex;
  justify-content: space-between;
  align-items: center;

}

.todo__content{
  display: flex;
}

.todo__completed{
  text-decoration: line-through;
  color: gray;
}

.todo__delete{
  cursor: pointer;
}

.todo__delete:hover{
  color: red;
}

.todo__content__title{
  padding: 3px;
}

.todo__content__input{
  font-size: 15.5px;
  background-color:rgb(228, 228, 228) ;
  }


.bottom__container{
  display: flex;
  justify-content: space-between;
  padding: 10px 0;
  margin: 30px 0;
  border-top: solid gray;

}

.active{
  background-color: rgb(104, 196, 104);
}



</style> 


