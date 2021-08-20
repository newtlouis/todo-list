<template>
  <div id="app" class="container">
    <img class="logo" src="./assets/logo.png">
    <TodoList/>
  </div>
</template>

<script>
import HelloWorld from './components/HelloWorld'
import TodoList from './components/TodoList.vue'

export default {
  name: 'App',
  components: {
    HelloWorld,
    TodoList
  }
}
</script
    TodoList>

<style>

*{
  box-sizing: border-box;
}

.container{
  max-width: 600px;
  margin: 0 auto;
  padding: 20px;
  background-color: rgb(228, 228, 228);
  box-shadow:10px 10px 10px grey;
}

#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin:60px auto;
}

.logo{
  display: block;
  margin: 20px auto;
  height: 75px;

}

</style>
